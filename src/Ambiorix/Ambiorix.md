# Ambiorix

# [Getting Started](https://gitlab.com/prpl-foundation/components/ambiorix/tutorials/getting-started#getting-started)

### Setup Git
```
git config --global user.email "EMAIL ID"
git config --global user.name "USER NAME"
```
### Install repo tool
```
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ./repo.sh
$ chmod a+x ./repo.sh
$ cp ./repo.sh /usr/local/bin/repo

$ repo init -u git://android.git.kernel.org/platform/manifest.git
$ repo sync
```
### Build AMX packages
- Source the scripts so these functions can be used:<br>
	`source .repo/manifests/scripts/local_commands.sh`

- To compile & install everything run `amx_rebuild_all`.
### Run Example
- Start the ubus daemon: `ubusd &`
-
Start the greeter example: `greeter -D`
- The output you will get should be similar to:
```
root@811203fdc548:~# ubus list
Greeter
Greeter.History
Greeter.History.1
Greeter.Statistics
```
# [Data Models](https://gitlab.com/prpl-foundation/components/ambiorix/tutorials/datamodels/introduction/-/blob/main/README.md#ambiorix-data-models)
### Data Model
A data model is a hierarchical set of Objects, Parameters, Commands and/or Events that define the managed Objects accessible via a particular Agent.

- Supported Data Model - this is the data model that is supported by the device (Agent). Basically this is a list of objects that is supported by the device.
- Instantiated Data Model - this is the data model currently in use by the device (Agent). This data model is used to configure the device or to query it's current state.
### Object
An internal node in the name hierarchy, i.e., a node that can have Object, Parameter, Command and/or Event children. An Object name is a Path Name.

There are different kind of objects in a data model:

- **singleton objects** - these objects are available (or not) and can not be instantiated.
- **multi-instance objects** - is an object that can have multiple instances, all of which are located at the same level within the name hierarchy. Multi-instance objects are also known as `template objects` or `table objects`
- **instance objects** - is an instance of a multi-instance object. Each instance is identified by an instance number and can be identified by an instance identifier (often the Alias).
#### Instance Number
A read-only positive integer (>=1) that uniquely identifies an instance within a Multi-Instance Object.
#### Instance Identifier

A value that uniquely identifies an instance within a Multi-Instance Object.

Often the instance identifier is the `Alias` (the name of the instance).

Instances can also be identified using one or more unique key parameters or using compound keys (multiple parameters of which the combined values are unique).
#### Parameter

A name-value pair that represents part of a CPE or USP Agent’s configuration or status. A Parameter name is a Path Name.

# [Ambiorix Data Models](https://gitlab.com/prpl-foundation/components/ambiorix/tutorials/datamodels/server/define-publish/-/blob/main/README.md#ambiorix-data-models---define-and-publish)

### Sections
An ODL file can contain multiple sections, each section has a specific role. The possible sections are:

- configuration section - this section contains configuration options, which can be application or service specific.
- define section - this section contains the data model definition.
- populate section - this section can be used to set entry point functions, event callback functions or set parameter values or add instances to multi-instance objects.

All of these sections can be used zero, 1 or more times in one single ODL file.

Each section starts with a specific keyword and must have a body.

- configuration sections: `%config`
- define sections: `%define`
- populate sections `%populate`

### Defining A Data Model Object
Defining an object is very easy and done using the keyword `object` followed by the object's name. Defining an object must be done in the `define` section.

```
%define {
    object Phonebook;
}
```
Using this notation a `singleton` object is defined.

### Adding Children
An object with only a name is not very useful.

As a phone book contains zero, one or more contacts, we need to add another object called `Contact` which is a child object of `Phonebook`.

As we may have multiple contacts in our phone book defining a singleton object is not sufficient, we need a `multi-instance` object.

```
%define {
    object Phonebook {
        object Contact[];
    }
}
```
Using brackets `[` and `]` after the object name indicates that it is a multi-instance object. Optionally you can specify the maximum number of instances that is allowed to be created within the brackets.

Example:

```
%define {
    object Phonebook {
        object Contact[10];
    }
}
```
### Adding Parameters

The initial object hierarchy is defined, but still we can not add any data to the objects like a name. What use does a contact has without a name?

Parameters can be added to any object, and is done in the object body. Parameters are key-value pairs and have a specific type. The parameter types possible are fixed and must be one of:

- string, csv_string, ssv_string
- uint8, uint16, uint32, uint64
- int8, int16, int32, int64
- bool
- datetime

So let us add the first and last name to the `Contact` multi-instance object.

```
%define {
    object Phonebook {
        object Contact[] {
            string FirstName;
            string LastName;
        }
    }
}
```
Names of objects, parameters, functions, events can be enclosed between single or double quotes.
```
%define {
    object "Phonebook" {
        object "Contact"[] {
            string "FirstName";
            string "LastName";
        }
    }
}
```
## Publish A Data Model
We defined a very simple data model, but till now it is not published.

To be able to publish a data model, we need an application that:

1. Opens and parses our odl file and builds the data model from it.
2. Connects to available software bus systems.
3. Implements an [event loop](https://en.wikipedia.org/wiki/Event_loop) and dispatches incoming requests and sends back replies.

We defined a very simple data model, but till now it is not published.

To be able to publish a data model, we need an application that:

1. Opens and parses our odl file and builds the data model from it.
2. Connects to available software bus systems.
3. Implements an [event loop](https://en.wikipedia.org/wiki/Event_loop) and dispatches incoming requests and sends back replies.

### Ambiorix Run Time
Let's use the Ambiorix container and publish the data model we have defined till now.

Start the container:
```
docker exec -ti -u $USER amxdev /bin/bash
export LD_LIBRARY_PATH=/usr/local/lib
$ ubusd &
```
#### Launch the data model as a daemon process:
```
# cat phonebook.odl
%define {
    object "Phonebook" {
        object "Contact"[] {
            string "FirstName";
            string "LastName";
        }
    }
}

# amxrt -D ./phonebook.odl

# ps -elf | grep amx
5 S root          65       1  0  80   0 -  1092 ep_pol 07:04 ?        00:00:00 amxrt -D ./phonebook.odl
```
Use `amxo-cg` to verify that the syntax is correct:
### Interact using Ubus
```
# ubus list
Phonebook
Phonebook.Contact
```
### Data Model Interaction

Although we did not define any functionality in our data model, the Ambiorix framework provides functions that can be used to interact with the data model.

The basic functions are:

- `_get`
- `_set`
- `_add`
- `_del`

> Note: These functions are prefixed with an underscore to prevent conflicts with other data model implementations that already provide these functions.

### Add A Contact
Using the `ubus` tool you can invoke the `default` data model methods:
```
$ ubus call Phonebook.Contact _add
{
        "object": "Phonebook.Contact.1.",
        "index": 1,
        "name": "1",
        "parameters": {
        },
        "path": "Phonebook.Contact.1."
}
```
After adding a contact you can `list` the available objects again:
```
$ ubus list
Phonebook
Phonebook.Contact
Phonebook.Contact.1
```
As you can see a new object has appeared `Phonebook.Contact.1`

#### Getting And Setting Parameters

#### Deleting An Instance Object

#### More advanced functions

### Interact Using PCB/BA-CLI
#### List The Objects
```
 - * - [bus-cli] (0)
 > ls
Phonebook.
```
You can query the full `phonebook` data model using the `?` query:
```
> Phonebook.?
Phonebook.
Phonebook.Contact.1.
Phonebook.Contact.1.FirstName=""
Phonebook.Contact.1.LastName=""
```
### Data Model Interaction

As `PCB/ba-cli` has some understanding of data models it already provides implementations of some of the available operations. These operations can be used directly using `pcb_cli` and are represented with a single symbol.

The basic functions are:

- `_get` - '?'
- `_set` - '='
- `_add` - '+'
- `_del` - '-'

#### Add A Contact

Adding a contact using `pcb_cli` can be done as follows:

```
> Phonebook.Contact.+
Phonebook.Contact.2
Phonebook.Contact.2.LastName=
Phonebook.Contact.2.FirstName=
```

After adding a contact you can `_get` the available objects again:

```
> Phonebook.?
Phonebook
Phonebook.Contact
Phonebook.Contact.2
Phonebook.Contact.2.LastName=
Phonebook.Contact.2.FirstName=
```

As you can see a new object has appeared `Phonebook.Contact.2`

It is also possible to create a new instance and provide all or some of the values of the parameters:

```
> Phonebook.Contact.+{LastName:"Smiths", FirstName:"Michael"}
```
#### Setting Parameters

The values of the individual parameters can be set using:

```
> Phonebook.Contact.2.LastName="Doe"
Phonebook.Contact.2.LastName=Doe
```

```
> Phonebook.Contact.2.FirstName="Jane"
Phonebook.Contact.2.FirstName=Jane
```

And verify again with the `_get` method using `?` in `pcb_cli`:

```
> Phonebook.?
Phonebook
Phonebook.Contact
Phonebook.Contact.2
Phonebook.Contact.2.LastName=Doe
Phonebook.Contact.2.FirstName=Jane
```
#### Deleting An Instance Object

Our `Jane Doe` contact can be deleted as well. Using the method `_del`. Deleting an instance object can be done in `pcb_cli` by using the `-` symbol behind the instance path.

```
> Phonebook.Contact.2.-
```

Check with `?` if the instance object is still available or not:

```
> Phonebook.?
Phonebook
Phonebook.Contact
```
#### Complete Phone book Instance
```
# ba-cli commands:

Phonebook.Contact.+
Phonebook.Contact.+

Phonebook.Contact.1.E-Mail.+
Phonebook.Contact.2.E-Mail.+
Phonebook.Contact.1.PhoneNumber.+
Phonebook.Contact.2.PhoneNumber.+

Phonebook.Contact.1.FirstName="Alpha"
Phonebook.Contact.1.LastName="Beta"
Phonebook.Contact.2.FirstName="Electric"
Phonebook.Contact.2.LastName="Energy"
Phonebook.Contact.1.PhoneNumber.1.Phone="+919876543211"
Phonebook.Contact.1.E-Mail.1.E-Mail="alphabeta@email.com"
Phonebook.Contact.2.PhoneNumber.1.Phone="+919363728361"
Phonebook.Contact.2.E-Mail.1.E-Mail="energy@email.com"

# ba-cli output:
 - * - [bus-cli] (0)
 > Phonebook.?
Phonebook.
Phonebook.Contact.1.
Phonebook.Contact.1.FirstName="Alpha"
Phonebook.Contact.1.LastName="Beta"
Phonebook.Contact.1.E-Mail.1.
Phonebook.Contact.1.E-Mail.1.E-Mail="alphabeta@email.com"
Phonebook.Contact.1.PhoneNumber.1.
Phonebook.Contact.1.PhoneNumber.1.Phone="+919625918617"
Phonebook.Contact.2.
Phonebook.Contact.2.FirstName="Electric"
Phonebook.Contact.2.LastName="Energy"
Phonebook.Contact.2.E-Mail.1.
Phonebook.Contact.2.E-Mail.1.E-Mail="energy@email.com"
Phonebook.Contact.2.PhoneNumber.1.
Phonebook.Contact.2.PhoneNumber.1.Phone="+919863798161"
```
# [Ambiorix Variants](https://gitlab.com/prpl-foundation/components/ambiorix/tutorials/collections/variants/-/blob/main/README.md#variants)
In `C` programming language all data types are always `fixed` types which can not change at run-time. This makes it hard to define functions or callbacks of which some of the arguments could be a different type depending on the situation. Typically in `C` programming this is solved by using `void *` and `casting`. The disadvantage of this technique is that you lose type information, which could lead to unwanted side effects.

Many other programming language have a `variant` type, this is a structure that can hold any type of data, without losing the data type information.

In `C` programming language the same functionality can be achieved by using a `tagged union`.

The `Ambiorix variant data container` is a tagged union and can contain many different types, each variant can hold only one data type at a given point in time.

### Supported Variant Types
The `Ambiorix variant` by default supports a set of data types, which can be separated into `primitive` and `composite` data types.

The default supported `primitive` data types are:

- null - `void` or `NULL`
- string - `char *`
- signed 8 bit integer - `int8_t`
- signed 16 bit integer - `int16_t`
- signed 32 bit integer - `int32_t`
- signed 64 bit integer - `int64_t`
- unsigned 8 bit integer - `uint8_t`
- unsigned 16 bit integer - `uint16_t`
- unsigned 32 bit integer - `uint32_t`
- unsigned 64 bit integer - `uint64_t`
- floating point - `float`
- double - `double`
- file descriptor `int`
- timestamp - `amxc_ts_t`
- boolean - `bool`

A `composite` data type is one that contains many variants, like a linked list of variants or a table containing key-value pairs where each value is a variant.

The default supported `composite` data types are:

- list - `amxc_llist_t` - contains a linked list of variants
- htable - `amxc_htable_t` - contains a hash table of variants, each item in the hash table has a key and a value. The value is a variant.

> **NOTE ABOUT STRING TYPES**
>
> Some special string types are supported as well: these are just strings, but as the variant type is different, it contains extra information. These types are:
>
> - csv_string - contains a string of `Comma Separated Values`
> - ssv_string - contains a string of `Space Separated Values`

> **NOTE ABOUT FILE DESCRIPTOR TYPES**
>
> A file descriptor in the Linux system is represented as an integer. When setting a file descriptor as a variant type, the variant implementation is verifying that the given integer is a valid file descriptor.

> **NOTE ABOUT TIMESTAMP TYPES**
>
> Timestamps are not a standard `C` type and are implemented in this library as a structure. The timestamp structure is considered a `primitive` as it does not contain multiple variants.

This set of `variant types` is the default set, it is possible to add your own `custom` types. How a custom type can be added is not handled in this document.
### Using Variants

#### Construct and Destruct a Variant

As with all `C` variables, you can define variants on the stack or allocate memory from the heap.

To define a local variant on the stack and initialize it:

```
amxc_var_t MyVariant;
amxc_var_init(&MyVariant);
```

To allocate memory from the heap:

```
amxc_var_t *pMyVariant = NULL;
amxc_var_new(&pMyVariant);
```

Both methods will create a variant containing a `null` type. It is also important to clean-up the variant when not needed anymore.

```
amxc_var_clean(&MyVariant);
amxc_var_delete(&pMyVariant);
```

The function `amxc_var_clean` will free all internally allocated memory (if any) to store the data, and will `reset` the variant back to the `null` type. This function can be used as well on variants that are allocated on the heap. After calling `amxc_var_clean` the variant variable is still usable.

The function `amxc_var_delete` will also free the memory that was allocated with `amxc_var_new` and resets the pointer to NULL.

#### Type Names and Type Identifiers

Each type has an identifier (integer) and a name (string). The default supported types have the following identifiers and names:

- ID = `AMXC_VAR_ID_NULL`, name = `null`
- ID = `AMXC_VAR_ID_CSTRING`, name = `cstring_t`
- ID = `AMXC_VAR_ID_INT8`, name = `int8_t`
- ID = `AMXC_VAR_ID_INT16`, name = `int16_t`
- ID = `AMXC_VAR_ID_INT32`, name = `int32_t`
- ID = `AMXC_VAR_ID_INT64`, name = `int64_t`
- ID = `AMXC_VAR_ID_UINT8`, name = `uint8_t`
- ID = `AMXC_VAR_ID_UINT16`, name = `uint16_t`
- ID = `AMXC_VAR_ID_UINT32`, name = `uint32_t`
- ID = `AMXC_VAR_ID_UINT64`, name = `uint64_t`
- ID = `AMXC_VAR_ID_FLOAT`, name = `float`
- ID = `AMXC_VAR_ID_DOUBLE`, name = `double`
- ID = `AMXC_VAR_ID_BOOL`, name = `bool`
- ID = `AMXC_VAR_ID_LIST`, name = `amxc_llist_t`
- ID = `AMXC_VAR_ID_HTABLE`, name = `amxc_htable_t`
- ID = `AMXC_VAR_ID_FD`, name = `fd_t`
- ID = `AMXC_VAR_ID_TIMESTAMP`, name = `amxc_ts_t`
- ID = `AMXC_VAR_ID_CSV_STRING`, name = `csv_string_t`
- ID = `AMXC_VAR_ID_SSV_STRING`, name = `ssv_string_t`

Depending on the function or macro used, the identifier must be specified, or the name.

> **NOTE ABOUT CUSTOM TYPES**
>
> Custom variant types can be defined. When such a custom type is `registered`, an identifier is assigned to it, the name is provided when registering the custom type. The chosen identifier can be different each time, therefore you need to fetch the identifier using the custom type's name. A function is available for this `amxc_var_get_type_id_from_name`. How to define and register your own custom type is out of the scope of this document and will be explained in a separate document.

#### Variant ID Implementation
**`libamx/include/amxc/amxc_variant.h:`**
```
#define AMXC_VAR_ID_INVALID      UINT32_MAX
#define AMXC_VAR_ID_NULL         0
#define AMXC_VAR_ID_CSTRING      1
#define AMXC_VAR_ID_INT8         2
#define AMXC_VAR_ID_INT16        3
#define AMXC_VAR_ID_INT32        4
#define AMXC_VAR_ID_INT64        5
#define AMXC_VAR_ID_UINT8        6
#define AMXC_VAR_ID_UINT16       7
#define AMXC_VAR_ID_UINT32       8
#define AMXC_VAR_ID_UINT64       9
#define AMXC_VAR_ID_FLOAT        10
#define AMXC_VAR_ID_DOUBLE       11
#define AMXC_VAR_ID_BOOL         12
#define AMXC_VAR_ID_LIST         13
#define AMXC_VAR_ID_HTABLE       14
#define AMXC_VAR_ID_FD           15
#define AMXC_VAR_ID_TIMESTAMP    16
#define AMXC_VAR_ID_CSV_STRING   17
#define AMXC_VAR_ID_SSV_STRING   18
#define AMXC_VAR_ID_ANY          19
#define AMXC_VAR_ID_CUSTOM_BASE  20
#define AMXC_VAR_ID_MAX          UINT32_MAX

#define AMXC_VAR_NAME_NULL       "null"
#define AMXC_VAR_NAME_CSTRING    "cstring_t"
#define AMXC_VAR_NAME_INT8       "int8_t"
#define AMXC_VAR_NAME_INT16      "int16_t"
#define AMXC_VAR_NAME_INT32      "int32_t"
#define AMXC_VAR_NAME_INT64      "int64_t"
#define AMXC_VAR_NAME_UINT8      "uint8_t"
#define AMXC_VAR_NAME_UINT16     "uint16_t"
#define AMXC_VAR_NAME_UINT32     "uint32_t"
#define AMXC_VAR_NAME_UINT64     "uint64_t"
#define AMXC_VAR_NAME_FLOAT      "float"
#define AMXC_VAR_NAME_DOUBLE     "double"
#define AMXC_VAR_NAME_BOOL       "bool"          /**< the bool variant name*/
#define AMXC_VAR_NAME_LIST       "amxc_llist_t"
#define AMXC_VAR_NAME_HTABLE     "amxc_htable_t"
#define AMXC_VAR_NAME_FD         "fd_t"
#define AMXC_VAR_NAME_TIMESTAMP  "amxc_ts_t"
#define AMXC_VAR_NAME_CSV_STRING "csv_string_t"  /**< the time stamp variant name*/
#define AMXC_VAR_NAME_SSV_STRING "ssv_string_t"  /**< the time stamp variant name*/
```
#### Variant Type Definition
```
/**
   @ingroup amxc_variant
   @brief
   The variant struct definition.

   A variant is a tagged union, which can be added to a linked list or to a
   hash table as value.

   Which field of the union is valid is depending of the type id field in the struct.
 */
typedef struct _amxc_var {
    amxc_llist_it_t lit;        /**< Linked list iterator, can be used to store the variant in a linked list */
    amxc_htable_it_t hit;       /**< Hash table iterator, can be used to store the variant in a hash table */
    amxc_var_type_id_t type_id; /**< Variant type: uint32_t */
    union
    {
        char* s;                /**< String */
        int8_t i8;              /**< 8 bit signed integer    */
        int16_t i16;            /**< 16 bit signed integer   */
        int32_t i32;            /**< 32 bit signed integer   */
        int64_t i64;            /**< 64 bit signed integer   */
        uint8_t ui8;            /**< 8 bit unsigned integer  */
        uint16_t ui16;          /**< 16 bit unsigned integer */
        uint32_t ui32;          /**< 32 bit unsigned integer */
        uint64_t ui64;          /**< 64 bit unsigned integer */
        float f;                /**< float   */
        double d;               /**< double  */
        bool b;                 /**< boolean */
        amxc_llist_t vl;        /**< ambiorix linked list of variants */
        amxc_htable_t vm;       /**< ambiorix hash table (key - value pair) of variants */
        int fd;                 /**< file descriptor */
        amxc_ts_t ts;           /**< time stamp */
        void* data;             /**< pointer to hold custom data types */
    } data;                     /**< Variant data */
} amxc_var_t;
```
#### Changing the Type

When a variant is instantiated, it is always a `null` variant. This type does not contain any data. Changing the type of a variant can be done using the function

```
int amxc_var_set_type(amxc_var_t * const var, const uint32_t type);
```

The type identifier (argument `type`) can be any of the type identifiers as mentioned in [Type Names and Type Identifiers](#type-names-and-type-identifiers)

When changing or setting the type of the variant, the current data stored in the variant is removed and the variant will contain the default value of the `type` set. The default value for integer, float, double is `0`, for booleans `false`, for timestamps `unknown time`, for strings `null string`, for list an empty linked list, for htable an empty hash table.

For primitive types it is not needed to first set the type and then set the value. The value can be set immediately after instantiating the variant.

#### Checking the Type

How to check that the type contained in a variant is the type that is expected?

To get the type identifier of a variant, use the function:

```
uint32_t amxc_var_type_of(const amxc_var_t * const var);
```

This function will return the type identifier of the contained data.

To fetch the name of the type contained in the variant, use:

```
const char *amxc_var_type_name_of(const amxc_var_t * const var);
```

Note that this function is returning a `const char *`, there is no need to free the returned pointer (it is not possible as it is `const`).

#### Converting And Casting Variants

Often you need the get the value of the variant in another type then how it is stored within the variant. Or you want to cast the variant to another type.

Requesting the value to be converted can be done by using `amxc_var_dyncast`. The macro `amxc_var_dyncast` is handled in more detail in section [Getting Primitive Values](#getting-primitive-values). To change the type and convert the value in the container to that type you can use `amxc_var_cast`.

Some types can be `automatically` casted to the best fitted type. For detailed information about this, consult the documentation of that variant type.

Example:

Assume you have a variant containing a string "1024", this can be casted to a `uint32_t` type.

```
amxc_var_t var;

amxc_var_init(&var);
amxc_var_set(cstring_t, &var, "1024");
amxc_var_cast(&var, AMXC_VAR_ID_UINT32);
```

Or use `autocast`

```
amxc_var_t var;

amxc_var_init(&var);
amxc_var_set(cstring_t, &var, "1024");
amxc_var_cast(&var, AMXC_VAR_ID_ANY);
```
#### Setting Values

##### Setting Primitive Values

Setting `primitive` values is easy and can be done by using the `amxc_var_set` macro. This macro can be used directly after instantiating the variant.

```
amxc_var_t my_var;

amxc_var_init(&my_var);
amxc_var_set(cstring_t, &my_var, "Hello World");
```

The macro takes as first argument the type name, as second the pointer to the variant, and as last argument the value. The value `type` must match the type mentioned as first argument.

The macro uses the type name and not the type identifier. The preprocessor will translate this macro into a function call.

```
int amxc_var_set_cstring_t(amxc_var_t * const var, const char * const val);
```
##### Setting Composite Values

Building a composite variant - this is a variant containing variants - requires a little bit more effort, by executing following steps:

- Instantiate the variant: `amxc_var_init`
- Set the correct `composite` type: `amxc_var_set_type`
- Fill the variant: use the macros `amxc_var_add` or `amxc_var_add_key`, depending on the composite type used.

To build a variant containing a linked list of variants

```
amxc_var_t my_var;

amxc_var_init(&my_var);
amxc_var_set_type(&my_var, AMXC_VAR_ID_LIST);
amxc_var_add(cstring_t, &my_var, "Hello");
amxc_var_add(cstring_t, &my_var, "World");
```
**Output:**
```
[
    "Hello",
    "World"
]
```

To build a variant containing a key - value pairs (hash table) where the values are variants. The keys are always strings.
```
amxc_var_t my_var;

amxc_var_init(&my_var);
amxc_var_set_type(&my_var, AMXC_VAR_ID_HTABLE);
amxc_var_add_key(cstring_t, &my_var, "key1", "Hello");
amxc_var_add_key(cstring_t, &my_var, "key2", "World");
```
**Output:**
```
{
    key1 = "Hello",
    key2 = "World"
}
```
It is possible to create a variant containing a linked list of variants, where each variant in the linked list is a variant containing key-value pairs.

You can now try [Practical Lab1](#lab1-build-a-composite-variant), or just continue reading. Before switching to the `Practical lab` first read [Practical Labs](#practical-labs).

## Getting Values

### Getting Primitive Values

Getting the values out of the variant can be done in two different ways:

1. Using macro `amxc_var_constcast`
2. Using macro `amxc_var_dyncast`

Both will retrieve the value of the variant, the difference is that `amxc_var_dyncast` will do conversion if needed and allocate memory if needed, while `amxc_var_constcast` will return the stored value that is in the variant as `const` if it is a pointer.

As `amxc_var_constcast` is never doing any conversions, it will return the default value of the requested type if the type contained in the variant is different.

Some examples:

- The variant contains a string and an integer is requested.
```
amxc_var_t my_var;
amxc_var_init(&my_var);
amxc_var_set(cstring_t, &my_var, "1234");
int32_t number = amxc_var_constcast(uint32_t, &my_var); // number will be 0
int32_t number = amxc_var_dyncast(uint32_t, &my_var); // number will be 1234
```
- The variant contains a boolean and a string is requested
```
amxc_var_t my_var;
amxc_var_init(&my_var);
amxc_var_set(bool, &my_var, true);
const char *txt = amxc_var_constcast(cstring_t, &my_var); // txt will be NULL
char *txt = amxc_var_dyncast(cstring_t, &my_var); // txt will be "true", txt must be freed when not needed anymore
```
### Getting Composite Values
#### Accessing Individual Primitives

Getting the individual values out of a composite variant can be done by accessing the individual variant by its `path`.

The function to retrieve a primitive variant out of the composite variant is:

```
amxc_var_t *amxc_var_get_path(const amxc_var_t * const var,
                              const char * const path,
                              const int flags);
```

This function will give the pointer to the variant as in the composite variant or a newly allocated variant, which you need to free with `amxc_var_delete`. The `flags` argument is used to indicate what you want, the variant as is or a copy. The flags can be:

- AMXC_VAR_FLAG_DEFAULT (no copy, use the variant as is)
- AMXC_VAR_FLAG_COPY (allocate a new variant, and copy the data)

> NOTE
>
> When using the default behavior `AMXC_VAR_FLAG_DEFAULT` the pointer to the variant in the composite variant is returned. When changing the value of that returned value, the value in the composite variant is changed.

**Example:**
Let's assume you have a list of users and for each user a user-id, name, and access rights.
Do not need to define a `fixed` structure, all you need is a variant that is initialized as a `list` type.

The function could look like (without error checking):
```
void add_user(amxc_var_t *users, uint32_t user_id, const char *name, uint32_t access_rights) {
    amxc_var_t *user = amxc_var_add(amxc_htable_t, users, NULL);
    amxc_var_add_key(uint32_t, user, "user_id", user_id);
    amxc_var_add_key(cstring_t, user, "name", name);
    amxc_var_add_key(uint32_t, user, "access_rights", access_rights);
}
```
Others can then use this function to fill the variant list
```
amxc_var_t *my_worker_function(void) {
    amxc_var_t *my_users = NULL;
    amxc_var_new(&my_users);
    amxc_var_set_type(my_users, AMXC_VAR_ID_LIST);
    add_user(my_users, 1, "John Doe", 0x755);
    add_user(my_users, 2, "Jane Doe", 0x644);
    add_user(my_users, 3, "Super", 0x777);
    return my_users;
}
```


Let's take the `users` data set (see [Setting Composite Values](#setting-composite-values)) and assume a complex variant is created with this data (JSON notation)

```
[
    {
        "user_id": 1,
        "name": "John Doe",
        "access_rights": 0x775
    },
    {
        "user_id": 2,
        "name": "Jane Doe",
        "access_rights": 0x664
    },
    {
        "user_id": 3,
        "name": "Super",
        "access_rights": 0x777
    }
]
```

Getting the name of the second user in the composite variant can be achieved by:

```
amxc_var_t *var_name = amxc_var_get_path(&users, "1.name", AMXC_VAR_FLAG_DEFAULT);
const char *str_name = amxc_var_constcast(cstring_t, var_name);
```

A list can be accessed using an index, the first element in a list will have index 0. Key-value pairs can be accessed by key.

###### Accessing The List

When a variant contains a `list` type, the pointer to the linked list in the variant can be fetched using the macro `amxc_var_constcast`.

```
const amxc_llist_t *var_list = amxc_var_constcast(amxc_llist_t, &my_var);
```

Use the `amxc_llist_t` API to access or iterate over the list. Each item in the list will be a variant. Use function `amxc_var_from_llist_it` to get the variant pointer.

###### Accessing The Hash Table

When a variant contains a `htable` type, the pointer to the hash table in the variant can be fetched using the macro `amxc_var_constcast`.

```
const amxc_htable_t *var_table = amxc_var_constcast(amxc_htable_t, &my_var);
```

Use the `amxc_htable_t` API to access or iterate over the hash table. Each item in the table will be a variant. Use function `amxc_var_from_htable_it` to get the variant pointer.

###### Iterating A Composite List

A composite variant containing a list of variants can be iterated using macro `amxc_var_for_each`

Example:

```
amxc_var_for_each(user, (&users)) {
    amxc_var_t *var_name = amxc_var_get_path(user, "name", AMXC_VAR_FLAG_DEFAULT);
    const char *str_name = amxc_var_constcast(cstring_t, var_name);
}
```
See: `kdev_print_ht_entries()`

> NOTE
>
> Iterating with the macro `amxc_var_for_each` works on both composite list variants and composite key-value pair variants.
>
> As an alternative you can get the pointer to the linked list embedded in the variant using `amxc_var_constcast(amxc_llist_t, &users)` and then use the linked list macro `amxc_llist_iterate` or `amxc_llist_for_each` to iterate over the linked list of variants.
>
> Or incase the variant contains a hash table use `amxc_var_constcast(amxc_htable, ...)` to get the embedded hash table and use the macro `amxc_htable_iterate` or `amxc_htable_for_each` to iterate over all items in the hash table.
>
> To get the variant pointer from a linked list iterator use macro `amxc_var_from_llist_it`. To get the variant pointer from a hash table use macro `amxc_var_from_htable_it`

#### Inspecting The Content

With very complex composite variants it can be difficult to understand what is stored in the variant. A debug function is available, that will print the content of the variant in a JSON like structure.

```
int amxc_var_dump(const amxc_var_t * const var, int fd);
```

The file descriptor can be STDOUT_FILENO, STDERR_FILENO or any other file descriptor with write permissions.

Example output (using the `users` example)

```
[
    {
        user_id: 1,
        name: "John Doe",
        access_rights: 0x775
    },
    {
        user_id: 2,
        name: "Jane Doe",
        access_rights: 0x664
    },
    {
        user_id: 3,
        name: "Super",
        access_rights: 0x777
    }
]
```
### [Practical Labs](https://gitlab.com/prpl-foundation/components/ambiorix/tutorials/collections/variants/-/blob/main/README.md#practical-labs)

## Example - amx-variant-contacts

A more complete example is available at
[https://gitlab.com/prpl-foundation/components/ambiorix/examples/collections/variant_contacts](https://gitlab.com/prpl-foundation/components/ambiorix/examples/collections/variant_contacts)

This example is documented and explained in the [README.md](https://gitlab.com/prpl-foundation/components/ambiorix/examples/collections/variant_contacts/-/blob/main/README.md) file.

# [Ambiorix Linked List](https://gitlab.com/prpl-foundation/components/ambiorix/tutorials/collections/linked-lists/-/blob/main/README.md#ambiorix-linked-list)

## Ambiorix Linked List API

The full API - structures, macros and functions - is documented.

The documentation can be consulted:

- In the header file [amxc_llist.h](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxc/-/blob/main/include/amxc/amxc_llist.h)
- As a web page [libamxc](https://prpl-foundation.gitlab.io/components/ambiorix/libraries/libamxc/doxygen)

### Linked List structures

The Ambiorix library `libamxc` contains two structures and a set of functions with which you can create data independent linked lists.

The first structure is the `linked list iterator` structure:

```
typedef struct _amxc_llist_it {
    struct _amxc_llist_it* next; /**< Pointer to the next item
                                      in the linked list */
    struct _amxc_llist_it* prev; /**< Pointer to the previous item
                                      in the linked list */
    struct _amxc_llist* llist;   /**< Pointer to the linked list */
} amxc_llist_it_t;
```

The second structure is the `linked list` main structure

```
typedef struct _amxc_llist {
    amxc_llist_it_t* head; /**< Pointer to the first item in the linked list */
    amxc_llist_it_t* tail; /**< Pointer to the last item in the linked list */
} amxc_llist_t;
```

Although it is possible to manipulate the members of these structures it is recommenced to use the API functions to do that.

### Linked List Functions

In this tutorial not all available linked list API functions will be handled, only the most commonly used.

#### Constructing and initializing a linked list

```
int amxc_llist_new(amxc_llist_t** llist);
int amxc_llist_init(amxc_llist_t* const llist);
```

When you need to allocate a linked list on the heap use `amxc_llist_new`, initializing a linked list on the stack is done by using `amxc_llist_init`. Please note that when allocating a linked list structure on the heap with `amxc_llist_new` it is already initialized, so there is no need to call `amxc_llist_init`.

#### Destructing and clean-up

```
void amxc_llist_delete(amxc_llist_t** llist, amxc_llist_it_delete_t func);
void amxc_llist_clean(amxc_llist_t* const llist, amxc_llist_it_delete_t func);
```

Items (nodes) in the linked list can be allocated on the heap, and therefore the allocated memory of those items must be freed when not needed anymore. Often when cleaning up the linked list itself, the items must be cleaned-up as well.

It is possible to create a loop that does exactly that, remove each item (node) from the linked list and free memory as needed. As this is a very common use case and mostly all items (nodes) in the linked list are of the same type, a `delete` callback function can be provided. This callback function will be called for each item (node) in the linked list.

#### Adding nodes

```
int amxc_llist_append(amxc_llist_t* const llist, amxc_llist_it_t* const it);
int amxc_llist_prepend(amxc_llist_t* const llist, amxc_llist_it_t* const it);
int amxc_llist_set_at(amxc_llist_t* llist, const unsigned int index, amxc_llist_it_t* const it);
int amxc_llist_it_insert_before(amxc_llist_it_t* const reference, amxc_llist_it_t* const it);
int amxc_llist_it_insert_after(amxc_llist_it_t* const reference, amxc_llist_it_t* const it);
```

All of these function will add an item (node) to the linked list, by using a pointer to the iterator that has been put in the data structure. If the item (node) was already in a list (the same or another one), it will be removed from the list it is in and added to the list at the requested position.

#### Removing nodes

```
void amxc_llist_it_take(amxc_llist_it_t* const it);
amxc_llist_it_t* amxc_llist_take_first(amxc_llist_t* const llist);
amxc_llist_it_t* amxc_llist_take_last(amxc_llist_t* const llist);
amxc_llist_it_t* amxc_llist_take_at(const amxc_llist_t* llist, const unsigned int index);
```

All of these functions will remove an item (node) from the linked list, but will not delete it from memory.

Keep in mind that it is not possible to directly address a node in a linked list by index. To get the node using an index in a in a linked list, the linked list needs to be traversed from the beginning until the node is reached that matches the given index.

#### Getting A Node

```
amxc_llist_it_t* amxc_llist_get_first(const amxc_llist_t* const llist);
amxc_llist_it_t* amxc_llist_get_last(const amxc_llist_t* const llist);
amxc_llist_it_t* amxc_llist_it_get_next(const amxc_llist_it_t* const reference);
amxc_llist_it_t* amxc_llist_it_get_previous(const amxc_llist_it_t* const reference);
amxc_llist_it_t* amxc_llist_get_at(const amxc_llist_t* const llist, const unsigned int index);
```

Keep in mind that it is not possible to directly address a node in a linked list by index. To get the node using an index in a linked list, the linked list needs to be traversed from the beginning until the node is reached that matches the given index.

#### Iterating All Nodes

```
#define amxc_llist_for_each(it, list)
#define amxc_llist_iterate(it, list)
```

These are macros that can be used to iterate over all nodes of a linked list.

Both macros should be used as a `for` loop:

```
amxc_llist_iterate(ll_it, my_linked_list) {
    // do something here
}
```

The differences between both macros is that with `amxc_llist_for_each` it is possible to remove the current `it` from the linked list, but it is not possible to embed another `amxc_llist_for_each` in the loop.

The macro `amxc_llist_iterate` doesn't allow that the linked list is modified while iterating, but it is possible to embed others `amxc_llist_iterate` in the loop.

# [Ambiorix Hash Table](https://gitlab.com/prpl-foundation/components/ambiorix/tutorials/collections/hash-tables/-/blob/main/README.md#ambiorix-hash-table)

A small phone book as a hash table:
![[Pasted image 20240705061929.png]]

## Hash Tables

### Generating a hash

Generating a hash from a key is very important when using a hash table. The hash is used to identify the bucket where the data is/can be stored. **A hash is calculated from a key and the result of the calculation should be an integer**. Using the integer modulo the number of buckets available provides the index of the bucket.

One of the problems here is that multiple keys could lead to the same index, this is called a collision. Collisions can be solved in different ways. One of them is using a linked list, and whenever a collisions occurs the new data is added to the end of the linked list.

The hash algorithm is also of importance when calculating the hash. Not all possible hash algorithms give a good distribution, depending on the keys used. To make a hash table implementation generic and re-usable, it must support the possibility to use a different hash algorithm.

The Ambiorix library `libamxc` already provides a set of hash algorithms, all implemented in a separate function.

The hash algorithms available are:

- [amxc_RS_hash](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxc/-/blob/main/src/amxc_hash_func.c#L63) - Sedgwicks Algorithms
- [amxc_JS_hash](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxc/-/blob/main/src/amxc_hash_func.c#L77) - Justin Sobel's hash function
- [amxc_PJW_hash](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxc/-/blob/main/src/amxc_hash_func.c#L88) - Peter J. Weinberger's Algorithm (AT&T Bell)
- [amxc_ELF_hash](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxc/-/blob/main/src/amxc_hash_func.c#L108) - [PJW Hash function (ELF hash)](https://en.wikipedia.org/wiki/PJW_hash_function)
- [amxc_BKDR_hash](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxc/-/blob/main/src/amxc_hash_func.c#L124) - Brian Kernighan and Dennis Ritchie's hash function
- [amxc_SDBM_hash](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxc/-/blob/main/src/amxc_hash_func.c#L136) - SDBM hash function from the SDBM project
- [amxc_DJB_hash](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxc/-/blob/main/src/amxc_hash_func.c#L147) - [Daniel J. Bernstein's hash algorithm](https://en.wikipedia.org/wiki/Daniel_J._Bernstein)
- [amxc_DEK_hash](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxc/-/blob/main/src/amxc_hash_func.c#L158) - DEK hash (Robert Sedgwicks Algorithms in C book)
- [amxc_BP_hash](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxc/-/blob/main/src/amxc_hash_func.c#L168) - BP hash (Donald E. Knuth in The Art Of Computer Programming Volume 3)
- [amxc_FNV_hash](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxc/-/blob/main/src/amxc_hash_func.c#L178) - FNV hash (Robert Sedgwicks Algorithms in C book)
- [amxc_AP_hash](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxc/-/blob/main/src/amxc_hash_func.c#L191) - AP hash (Arash Partow)

You can look at the implementations of these hash function in the file [amxc_hash.c](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxc/-/blob/main/src/amxc_hash_func.c)

Ofcourse you can implement your own hash function that provides maybe a better distribution for your case.

The function can be used by the Ambiorix hash table implementation as long as it fits this signature:

```
typedef unsigned int (* amxc_htable_hash_func_t) (const char* key, const unsigned int len);
```

After creation or initialization of the hash table you can change the hash function used for that table by calling:

```
void amxc_htable_set_hash_func(amxc_htable_t* const htable, amxc_htable_hash_func_t func);
```

> Note
> Do not change the hash function when the hash table already contains items. Changing the hash function does not recalculate all indexes for the items in the table.
>
> Only set the hash function after creating or initializing the hash table.

### Number Of Buckets

The number of buckets available is also important, as it can have some influence on the efficiency of the hash table. When the number of buckets is too small, the number of possible collisions can get high. When the number of buckets is too big, the memory overhead of the hash table increases (many empty buckets).

It is not always possible to predict the number of items that will be stored in the hash table, so it is important that the size of the hash table (number of available buckets) can be modified, manually or automatically.

When the number of items in the Ambiorix hash table exceeds 75% of the number of available buckets, the number of buckets is increased. When this happens the index of each item in the hash table is recalculated and the item can get re-positioned.

### What can be put in a hash table?

Any data structure can be added to the hash table, the only limitation is that the key always must be a string.

To make a data structure ready to be used with a hash table it must contain a `amxc_htable_it_t` member. This is the `hash table iterator`

Example:

```
typedef struct person {
    char* first_name;
    char* last_name;
    gender_t gender;
    uint32_t age;
    char* email;
    char* phone;
    marital_status_t mstatus;
    amxc_htable_it_t it;
} person_t;
```

All Ambiorix hash table API functions work either with a `amxc_htable_t` or a `amxc_htable_it_t`. Fetching an item from the hash table will give you a pointer to `amxc_htable_it_t`, using this pointer and `offsetof` macro (through `amxc_container_of`), the address of the containing structure is calculated.
## Ambiorix Hash Table API

The full hash table API - structures, macros and functions - is documented.

- In the header file [amxc_htable.h](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxc/-/blob/main/include/amxc/amxc_htable.h)
- As a [web page](https://soft.at.home.gitlab.io/ambiorix/libraries/libamxc/doxygen)

### Hash Table structures

The Ambiorix library `libamxc` contains two structures and a set of functions with which you can create data independent linked lists.

The first structure is the `hash table iterator` structure:

```
struct _amxc_htable_it {
    amxc_array_it_t* ait;
    char* key;
    amxc_htable_it_t* next;
};
```

The second structure is the `hash table` main structure:

```
typedef struct _amxc_htable {
    amxc_array_t table;
    size_t items;
    amxc_htable_hash_func_t hfunc;
    amxc_htable_it_delete_t it_del;
} amxc_htable_t;
```

Although it is possible to manipulate the members of these structures it is recommended to use the API functions to do that.

### Hash Table Functions

In this tutorial not all available hash table API functions will be handled, only the most commonly used.

#### Constructing and Initializing a hash table

```
int amxc_htable_new(amxc_htable_t** htable, const size_t reserve);
int amxc_htable_init(amxc_htable_t* const htable, const size_t reserve);
```

When you need to allocate a hash table on the heap use `amxc_htable_new`, initializing a hash table on the stack is done by using `amxc_htable_init`. **Please note that when allocating a hash table structure on the heap with `amxc_htable_new` it is already initialized, so there is no need to call `amxc_htable_init`.**

When creating a hash table, the size of the table must be specified. This size will be used to allocate the buckets. A hash table in Ambiorix is not fixed in size, when adding more items in the hash table the hash table will grow in size when needed.

#### Destructing and clean-up

```
void amxc_htable_delete(amxc_htable_t** htable, amxc_htable_it_delete_t func);
int amxc_htable_clean(amxc_htable_t* const htable, amxc_htable_it_delete_t func);
```

Items in the hash table can be allocated on the heap, and therefor the allocated memory of those items must be freed when not needed anymore. Often when cleaning up the hash table itself, the items must be cleaned-up as well.

It is possible to create a loop that does exactly that, remove each item from the hash table and free memory as needed. As this is a very common use case and mostly all items in the hash table are of the same type, a `delete` callback function can be provided. This callback function will be called for each item in the hash table.

#### Adding An Item

```
int amxc_htable_insert(amxc_htable_t* const htable, const char* const key, amxc_htable_it_t* const it);
```

Using this function a key-value pair is added to the hash table, the hash table iterator pointer `amxc_htable_it_t` is pointing to the item (data). Each iterator can only be added to a hash table once. Setting the same iterator to another hash table will result in removal from the first hash table.

Hash tables allow duplicate keys, so the keys do not need to be unique. But keep in mind that adding many duplicate keys in the hash table, will reduce the efficiency of the hash table.

#### Getting An Item

```
amxc_htable_it_t* amxc_htable_get(const amxc_htable_t* const htable, const char* const key);
amxc_htable_it_t* amxc_htable_it_get_next_key(const amxc_htable_it_t* const reference);
```

Using the key you can get an `hash table iterator` from the hash table itself. When no item is stored with the given key, this function will return a NULL pointer. When items with duplicate keys are stored in the hash table, the first occurrence is returned. To get the next item with the same key, use `amxc_htable_it_get_next_key`.

#### Removing An Item

```
amxc_htable_it_t* amxc_htable_take(amxc_htable_t* const htable, const char* const key);
```

This function will remove the first item with the given key from the hash table, but will not delete the item. The item's hash table iterator is returned. Use `amxc_container_of` macro to get the pointer to the data structure.

Be careful when the hash table contains duplicate keys, it will not remove all items with that key, just the first one.

#### Iterating All Items

```
#define amxc_htable_for_each(it, htable)
#define amxc_htable_iterate(it, htable)
```

These are macros that can be used to iterate over all items in a hash table.

Keep in mind that iterating over all items in a hash table is not very efficient. When using these macros you are accessing the hash table like a linked list.

Both macros should be used as a `for` loop:

```
amxc_htable_for_each(ht_it, my_hash_table) {
    // do something here
}
```

The differences between both macros is that with `amxc_htable_for_each` it is possible to remove the current `it` from the hash table, but it is not possible to embed another `amxc_htable_for_each` in the loop.

The macro `amxc_htable_iterate` doesn't allow that the hash table is modified while iterating, but it is possible to embed others `amxc_llist_iterate` in the loop.

### Lab1 - [Search a word in a dictionary](https://gitlab.com/prpl-foundation/components/ambiorix/tutorials/collections/hash-tables/-/blob/main/README.md#lab1---search-a-word-in-a-dictionary)
`/root/Workspace/tutorials/hash-tables/labs/lab1/main.c`
### Lab2 - [Iterating a hash table](https://gitlab.com/prpl-foundation/components/ambiorix/tutorials/collections/hash-tables/-/blob/main/README.md#lab2---iterating-a-hash-table)
`/root/Workspace/tutorials/hash-tables/labs/lab2/main.c`
### Lab3 -[Hash table of Contacts](https://gitlab.com/prpl-foundation/components/ambiorix/tutorials/collections/hash-tables/-/blob/main/README.md#lab3---hash-table-of-contacts)
`/root/Workspace/tutorials/hash-tables/labs/lab3/main.c`

# [Ambiorix Data Models - Simple Client](https://gitlab.com/prpl-foundation/components/ambiorix/tutorials/datamodels/client/simple-client/-/blob/main/README.md#ambiorix-data-models---simple-client)

## Introduction

Services and applications can `publish` a data model that can act as the public interface of these services and applications. Typically these data models are `published` through a software bus system and provide configuration options or the current status of these services and applications. Other applications (aka clients), can interact with them. The interaction is done using `standard` operators like get, set, add, delete, ...

Interacting with a `public` data model can either be done using the `native` bus system tools and API. A disadvantage of using the `native` API is that your `client` application is not exchangeable between different `Software Platforms`, so you need to `port` your application and provide support for all possible `Software Platforms`. This can lead into maintenance hell.

The Ambiorix framework provides an API that can work on top of many bus systems, if an `Ambiorix Bus Back-end` is implemented for that bus system. Using this API (aka BAAPI), will make your application exchangeable between different `Software Platforms` or `Distributions`.

## BAAPI - libamxb API

The full API - structures, macros and functions - is documented.

The documentation can be consulted:

- In the header files
    - [amxb_connect.h](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxb/-/blob/main/include/amxb/amxb_connect.h)
    - [amxb_operators.h](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxb/-/blob/main/include/amxb/amxb_operators.h)
- As a [web page](https://soft.at.home.gitlab.io/ambiorix/libraries/libamxb/doxygen)

### Loading Back-ends

Ambiorix by itself does not provide a protocol or messaging system, it provides an API that is protocol or message independent and relies on a `back-end` to send and receive messages.

The `Bus Agnostic API` provides API's that allow you to load one or more back-ends or even to unload them. These API's don't implement some magical thing, they just use a standard mechanism known as [Dynamically Loaded (DL) Libraries](https://tldp.org/HOWTO/Program-Library-HOWTO/dl-libraries.html)

To load a bus/protocol back-end use:

```
int amxb_be_load(const char* path_name);
```

The `path_name` argument must be an absolute or relative path including the filename and extension to a shared object file that implements an Ambiorix BAAPI back-end interface. When the shared object is loaded using [dlopen](https://linux.die.net/man/3/dlopen), some checks are performed:

1. Does this shared object implement a BAAPI back-end interface?
2. Is the implemented interface compatible with the interface version in the currently used libamxb?
3. Are all mandatory functions available?

If any of these checks fail, the shared object will be unloaded again and can not be used as a back-end.

Multiple back-ends can be loaded at a single point in time.

To unload or remove a back-end use:

```
int amxb_be_remove(const char* backend_name);
void amxb_be_remove_all(void);
```

The function `amxb_be_remove_all` is typically used when the application is exiting.

Also note that when removing a back-end, all connections created with that back-end will be closed.

Example code:
```
    const char* ba_backend = getenv("AMXB_BACKEND");
    int retval = amxb_be_load(ba_backend);
    if (retval != 0) {
        printf("Failed to load back-end [%s]\n", ba_backend);
        printf("Return code = %d\n", retval);
        exit(2);
    }
```
### Creating A Connection

To be able to communicate with other processes a `connection` must be created to them, to make it possible to exchange messages. A similar mechanism is used when you access a web-page in your browser. Your browser will translate the provided URI into an IP address, creates a connection with the web-server using this IP address and then your browser starts exchanging messages with the server (using the HTTP on top of TCP/IP).

Such a connection between two endpoints is called a socket. With a socket bi-directional communication is possible.

Simplified this can be expressed with this simple drawing:

![[Pasted image 20240705195209.png]]

Here two applications are connected to each using a TCP/IP socket. The same can be achieved when the two applications are running on the same machine, they can set-up a connection to each other using a TCP/IP socket and use the `local host` address.

In Linux (or unix like systems) another kind of socket can be created between two applications, these sockets are called `Unix Domain Sockets`.

Most Software platforms (openWrt, prplWrt, SAH sop, ...) also provide a kind of message broker/dispatcher process which is mostly referred to as the `software bus`. The responsibility of such a `software bus` is dispatching the messages to the correct application or service. Often these `bus systems` use Unix Domain sockets.

![[Pasted image 20240705195237.png]]

The advantage of having a `software bus` in between is that you only need one single socket to be able to communicate and exchange messages between your application and all other applications connected to the same `bus system`.

The Ambiorix `Bus Agnostic API`, implemented in libamxb, provides functions that help in creating connections between your application and the `bus system` or between individual applications, or closing these connections when you don't need them anymore.

```
int amxb_connect(amxb_bus_ctx_t** ctx, const char* uri);
int amxb_disconnect(amxb_bus_ctx_t* ctx);
```

To establish a connection you need a [Uniform Resource Identifier](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier) or short a URI.

Examples of URIs are:

- [http://www.google.com](http://www.google.com)
- [ftp://myftp.ftp-service.com](ftp://myftp.ftp-service.com/)

And you probably already used these kind of URIs when browsing the world wide web using your favorite browser.

The full syntax of a URI is:

```
scheme:[//authority]path[?query][#fragment]
```

The function `amxb_connect` needs such a URI to know how to create the socket and most of all to know which back-end must be used.

The `scheme` of the URI is used to identify the back-end.

For instance, if you want to connect to `ubusd` (on openWrt), you need a URI that starts with `ubus:`. If you want to connect to `pcb_sysbus` you need a URI that starts with `pcb:`

Looking at the syntax of a URI, you will see that only two parts are mandatory:

- scheme
- path

A `Unix Domain sockets` is represented by a file in the file system, which has a path (including a file name).

The URIs for `ubus` and `PCB` can then be written as:

```
ubus:/var/run/ubus/ubus.sock
pcb:/var/run/pcb_sys
```

> **NOTE**
> Depending on the `distribution` used or `build` used, these `Unix Domain` sockets can have other names or be in different directories

Example code:
```
    amxb_bus_ctx_t* bus_ctx = NULL;
    int retval = amxb_connect(&bus_ctx, "ubus:/var/run/ubus/ubus.sock");
    if(retval != 0) {
        printf("Failed to connect to [%s]\n", uri);
        printf("Return code = %d\n", retval);
        exit(3);
    }
```
### Data Model Operators

To interact with an application or service that exposes a data model, more is needed than only a connection. We must be able to send `requests` or messages to these applications or services.

The [TR-369 USP Broadband Forum specification](https://www.broadband-forum.org/download/TR-369.pdf) describes operations that can be performed on a data model, the basic operations are:

- add - adds an instance to a multi-instance (table) object.
- delete - deletes an instance from a multi-instance (table) object.
- set - changes values of parameters.
- get - requests the current values of parameters of one or more objects.

For each of these operations a function is available in the `Bus Agnostic API` and is implemented in libamxb:

```
int amxb_add(amxb_bus_ctx_t* const bus_ctx, const char* object, uint32_t index, const char* name, amxc_var_t* values, amxc_var_t* ret, int timeout);
int amxb_del(amxb_bus_ctx_t* const bus_ctx, const char* object, uint32_t index, const char* name, amxc_var_t* ret, int timeout);
int amxb_set(amxb_bus_ctx_t* const bus_ctx, const char* object, amxc_var_t* values, amxc_var_t* ret, int timeout);
int amxb_get(amxb_bus_ctx_t* const bus_ctx, const char* object, int32_t depth, amxc_var_t* ret, int timeout);
```

All of these functions take as first argument a `bus context`, this is the connection you have created using `amxb_connect`.

The second argument is a path and is referring to one or more objects in the `remote` data model, depending on the operator this can be one of:

- Object path: This is a `Path Name` of either a singleton Object, or the `Path Name` to an multi-instance Object. An Object Path ends in a “.”
- Object Instance Path - This is a `Path Name` to a an instance object of a multi-instance object. It uses an `Instance Identifier` to address a particular Instance of the multi-instance object. An `Object Instance Path` ends in a “.”
- Parameter Path - This is a `Path Name` of a particular parameter of an object.
- Search Path - This is a `Path Name` that contains search criteria for addressing a set of instance objects and/or their Parameters. A Search Path may contain a `Search Expression` or `Wildcard`.

All of the functions have a `ret` argument, which is a `Ambiorix Variant` and will contain the response if the function execution did not fail.

# [Ambiorix Data Models - Subscriptions](https://gitlab.com/prpl-foundation/components/ambiorix/tutorials/datamodels/client/subscriptions#ambiorix-data-models---subscriptions)

## Introduction

When using a modular system, different processes can provide their own data model. **However one process might be interested in data model changes of another process. This information can be obtained by sending out events and subscribing to these events.**
## Different types of events

The Ambiorix data model already provides a set of default events. These default events are automatically sent when the data model is updated using a `transaction`. Transactions will be discussed in a different tutorial, but you can see **a transaction as a group of multiple actions that are executed at once. If one of these actions fails, nothing should be changed**. The full documentation on transactions can be found in [Actions, Transactions & Events](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxd/-/blob/main/doc/actions_transactions_events.md).

The default events are explained in the [ODL documentation](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxo/-/blob/main/doc/odl.md#define-events), but are repeated here for clarity:

- `dm:root-added` - send when a new `root` object has been added
- `dm:root-removed` - send when a `root` object has been deleted
- `dm:object-added` - send when an object has been added in the hierarchical tree
- `dm:object-removed` - send when an object has been removed from the hierarchical tree
- `dm:instance-added` - send when an instance has been created
- `dm:instance-removed` - send when an instance has been deleted
- `dm:object-changed` - send when an object has been changed
- `dm:periodic-inform` - send at a regular interval, only when periodic-inform has been enabled
- `app:start` - The application should send this event when the data model is loaded
- `app:stop` - The application should send this event when the data model is going to be removed.

Besides these events, it is also possible to create object-defined events. These events must be defined in the `object definition body`.

### ODL event syntax

Syntax:
```
event "<name>";
```
A concrete example of such an event would be a [Device.LocalAgent.Threshold.{i}.Triggered!](https://usp-data-models.broadband-forum.org/tr-181-2-13-0-usp.html#D.Device:2.Device.LocalAgent.Threshold.%7Bi%7D.Triggered!) event. This event must be send out when a focused parameter reached a given value.

Sending out an object-defined event can be done by using

```
void amxd_object_emit_signal(amxd_object_t* const object, const char* name, amxc_var_t* const data);
```

This is considered as an advanced feature and will not be discussed in this tutorial. If you are interested in this functionality, you can have a look at the [cpu-info example](https://gitlab.com/prpl-foundation/components/ambiorix/examples/datamodel/cpu-info). Here we will focus on the default events instead.

## What does an ambiorix event looks like

When an ambiorix event is triggered, a variant is sent out containing the event data. Depending on the back-end you are using, this variant can be converted to a back-end specific message before it is send out over the bus. At the receiving side, this message is converted back to a variant. The nice thing about this system is that you don't have to care about how the data is transported. You just want to be able to receive events.

An event variant is a hash table of data. Depending on which event was triggered, different information can be available in the table, but it will always contain at least 3 entries:

- The `notification` type, for example `dm:object-added`
- The `object` the notification belongs to, for example `Phonebook.Contact.contact-1.PhoneNumber.`
- The `path` to the object, for example `Phonebook.Contact.1.PhoneNumber.`

The difference between the `object` and `path` is that an `object` will contain the instance name (Alias) if it has one, while this Alias is replaced by an index for the `path`. In case of the Phonebook example, there is no parameter Alias, so the `object` name will be the same as the `path`: `Phonebook.Contact.1.Phonenumber.`.

Here you can see the corresponding variant:

```
{
    notification = "dm:object-added",
    object = "Phonebook.Contact.1.PhoneNumber.",
    path = "Phonebook.Contact.1.PhoneNumber."
}
```

Depending on which event was sent, other information can also be present in the event variant. For example when adding an instance of `Contact` to the `Phonebook` object, you will get the following event:

```
{
    index = 1,
    keys = {
    },
    name = "1",
    notification = "dm:instance-added",
    object = "Phonebook.Contact.",
    parameters = {
        FirstName = ""
        LastName = "",
    },
    path = "Phonebook.Contact."
}
```

Besides the `notification`, `object` and `path`, the event also has entries for the instance `index`, its `parameters` at creation time and its `keys`. The `key parameters` of an instance uniquely identify that instance. In other words, you can never have 2 instances with the same key parameters. More on this is explained in the odl [documentation](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxo/-/blob/main/doc/odl.md) and in the tutorial [Object And Parameter Validation](https://gitlab.com/prpl-foundation/components/ambiorix/tutorials/datamodels/server/validation).

## Ambiorix subscription functions

Subscribing to events and unsubscribing from events from an object can be done with

```
int amxb_subscribe(amxb_bus_ctx_t* const ctx,
                   const char* object,
                   const char* expression,
                   amxp_slot_fn_t slot_cb,
                   void* priv);

int amxb_unsubscribe(amxb_bus_ctx_t* const ctx,
                     const char* object,
                     amxp_slot_fn_t slot_cb,
                     void* priv);
```

How it works is explained in the [documentation](https://soft.at.home.gitlab.io/ambiorix/libraries/libamxb/master/dc/d54/a00094.html#ga9ad6c052f5f2494b73c5d8f296729ef4). The callback function that is called when an event is received has the following prototype:

```
typedef void(* amxp_slot_fn_t) (const char *const sig_name, const amxc_var_t *const data, void *const priv)
```

Its arguments are also explained in the [documentation](https://soft.at.home.gitlab.io/ambiorix/libraries/libamxp/master/d2/d79/a00051.html#ga26f72e3fe02d7ba95264f44c9b7d3c6f). The passed private data is the same data that was provided when subscribing.

The passed `expression` when subscribing can be used to filter out certain events. A few examples are given here to show which kinds of expressions are valid. Suppose you are subscribing to events from the `Phonebook.Contact.` object. By default you will receive all events that are sent out from this object. If you are only interested in events when `Contact` objects are added or removed, you could apply a filter such as:

```
const char* expression = "notification in ['dm:instance-added','dm:instance-removed']"
```

This will filter out all events with a different notification. If you are only interested in notifications when a certain parameter is set, you could add this to the expression as well:

```
const char* expression = "notification in ['dm:instance-added','dm:instance-removed'] && parameters.FirstName == 'Ella'"
```

Now you will only receive events if an instance is added or removed and the parameter `FirstName` is equal to `Ella`. It is also possible to expand this search path with wildcards. Suppose you want to be notified of all additions or removals where the `FirstName` starts with `Ell`, then you can use the `matches` keyword and as a value pass a [posix extended regular expression](https://www.gnu.org/software/findutils/manual/html_node/find_html/posix_002dextended-regular-expression-syntax.html):

```
const char* expression = "notification in ['dm:instance-added','dm:instance-removed'] && parameters.FirstName matches 'Ell\.*'"
```

Now you will also get events when a `Contact` named `Elliot` is added.

### Lab1 - Interactive subscribe example

There already exists an example subscription client for Ambiorix and we will use it in this lab. This example can be found [here](https://gitlab.com/prpl-foundation/components/ambiorix/examples/baapi/subscribe).

It explains how to subscribe to events using a command line application. The README file explains how to subscribe to events from the greeter plugin using ubus. In this lab we will subscribe to events from our Phonebook applications using PCB.


> Note: you can also make this lab using ubus. Which ubus commands to execute are not listed here, but the output you receive from amx-subscribe should be the same.

Let's create a directory for the example and clone it there.

> **NOTE**
> 
> If you followed the Getting Started tutorial, all examples should already be available in your workspace directory, if it is already available, you can skip cloning the repository.

```
mkdir -p ~/workspace/ambiorix/examples/baapi
cd ~/workspace/ambiorix/examples/baapi
git clone git@gitlab.com:prpl-foundation/components/ambiorix/examples/baapi/subscribe.git
```

You can build the applications using

```
cd ~/workspace/ambiorix/examples/baapi/subscribe
make
sudo -E make install
```

The application uses `AMXB_BACKEND` as an environment variable to determine the back-end that will be used. Since we are using PCB in this lab, we will set it to

```
export AMXB_BACKEND=/usr/bin/mods/amxb/mod-amxb-pcb.so
```

Before continuing, switch to the `lab1` folder and launch the `phonebook.odl` using amxrt

```
cd ~/workspace/ambiorix/tutorials/datamodels/client/subscriptions/labs/lab1
amxrt -D phonebook.odl
```

Now you can subscribe to events using the subscribe application:

```
amx-subscribe <URI> <OBJECT> [<EXPRESSION>]
```

- URI - the uri to connect to, for pcb this is normally pcb:/var/run/pcb_sys
- OBJECT - the object path of the `data model` object of which you want to receive events.
- EXPRESSION - (optional) an event filter expression.

We will listen to events from the Phonebook object

```
$ amx-subscribe pcb:/var/run/pcb_sys Phonebook. 
```

When launched nothing will happen, the `amx-subscribe` application is now waiting for events. All you need to do is make sure the `phonebook` is generating events on the object `Phonebook.`

So open a new console and run `pcb_cli`. First check that the `Phonebook.` object can be found

```
> Phonebook.?
Phonebook
Phonebook.Contact
```

Then add a new Contact

```
> Phonebook.Contact.+
Phonebook.Contact.1
Phonebook.Contact.1.LastName=
Phonebook.Contact.1.FirstName=
```

Now you should see incoming events in the terminal where you subscribed to events.

```
Notification received [Phonebook]:
{
    index = 1,
    keys = {
    },
    name = "1",
    notification = "dm:instance-added",
    object = "Phonebook.Contact.",
    parameters = {
        FirstName = ""
        LastName = "",
    },
    path = "Phonebook.Contact."
}
Notification received [Phonebook]:
{
    notification = "dm:object-added",
    object = "Phonebook.Contact.1.PhoneNumber.",
    path = "Phonebook.Contact.1.PhoneNumber."
}
Notification received [Phonebook]:
{
    notification = "dm:object-added",
    object = "Phonebook.Contact.1.E-Mail.",
    path = "Phonebook.Contact.1.E-Mail."
}
```

The `amx-subscribe` that is running, will print all events from all objects under the `Phonebook` object. Using the `expression` feature of the `Ambiorix` framework , it is possible to easily filter out the events you want to see.

Now stop the `amx-subscribe` (press CTRL+C in the console where it is running) application and relaunch it:

```
amx-subscribe pcb:/var/run/pcb_sys Phonebook.Contact. "notification in ['dm:instance-added','dm:instance-removed'] && parameters.FirstName == 'Ella'"
```

Add a new contact named `John`

```
> Phonebook.Contact.+{FirstName="John"}
Phonebook.Contact.2.FirstName=John
```

This should not generate any event. Add a new contact named `Ella`

```
> Phonebook.Contact.+{FirstName="Ella"}
Phonebook.Contact.3.FirstName=Ella
```

Now you will get an event because the subscribe expression matches the event

```
Notification received [Phonebook.Contact]:
{
    index = 3,
    keys = {
    },
    name = "3",
    notification = "dm:instance-added",
    object = "Phonebook.Contact.",
    parameters = {
        FirstName = "Ella"
        LastName = "",
    },
    path = "Phonebook.Contact."
}
```

Similarly if you remove both contacts, you will only receive an event for deleting the contact named `Ella`.

```
> Phonebook.Contact.2.-
> Phonebook.Contact.3.-
```

```
Notification received [Phonebook.Contact]:
{
    index = 3,
    keys = {
    },
    name = "3",
    notification = "dm:instance-removed",
    object = "Phonebook.Contact.",
    parameters = {
        FirstName = "Ella"
        LastName = "",
    },
    path = "Phonebook.Contact."
}
```

### Lab1 - Test Log

Getting below error even the backend is exported:
```
$ amx-subscribe ubus:/var/run/ubus/ubus.sock Phonebook.
 
ERROR -- No backends specified
```

Below workaround can be done:
```
amx-subscribe -B /usr/bin/mods/amxb/mod-amxb-ubus.so  -u ubus:/var/run/ubus/ubus.sock  Phonebook.
```

Test log:
```
[root@ambiorix]:~/.../subscriptions/labs/lab1$ 
[root@ambiorix]:~/.../subscriptions/labs/lab1$ amxrt -D ./phonebook.odl 
[root@ambiorix]:~/.../subscriptions/labs/lab1$ 
[root@ambiorix]:~/.../subscriptions/labs/lab1$ ubus list
Phonebook
Phonebook.Contact
[root@ambiorix]:~/.../subscriptions/labs/lab1$
[root@ambiorix]:~/.../subscriptions/labs/lab1$
[root@ambiorix]:~/.../subscriptions/labs/lab1$ amx-subscribe -B /usr/bin/mods/amxb/mod-amxb-ubus.so  -u ubus:/var/run/ubus/ubus.sock  Phonebook. &
Wait until object [Phonebook.] is available
Wait done - object [Phonebook.] is available

[root@ambiorix]:~/.../subscriptions/labs/lab1$ 
[root@ambiorix]:~/.../subscriptions/labs/lab1$ ba-cli Phonebook.Contact.+ 
> Phonebook.Contact.+
Phonebook.Contact.1.
Notification received from [Phonebook]:
{
    eobject = "Phonebook.Contact.",
    index = 
1,
    keys = {
    },
    name = "1",
    notification = "dm:instance-added",
    object = "Phonebook.Contact.",
    parameters = {
        FirstName = "",
        LastName = ""
    },
    path = "Phonebook.Contact."
}
Notification received from [Phonebook]:
{
    eobject = "Phonebook.Contact.[1].PhoneNumber.",
    notification = "dm:object-added",
    object = "Phonebook.Contact.1.PhoneNumber.",
    parameters = {
        Phone = ""
    },
    path = "Phonebook.Contact.1.PhoneNumber."
}
Notification received from [Phonebook]:
{
    eobject = "Phonebook.Contact.[1].E-Mail.",
    notification = "dm:object-added",
    object = "Phonebook.Contact.1.E-Mail.",
    parameters = {
        E-Mail = ""
    },
    path = "Phonebook.Contact.1.E-Mail."
}
```

Re-launch with filter:
```
$ amx-subscribe -B /usr/bin/mods/amxb/mod-amxb-ubus.so  -u ubus:/var/run/ubus/ubus.sock  Phonebook. "notification in ['dm:instance-added','dm:instance-removed'] && parameters.FirstName == 'Ella'" &
Wait until object [Phonebook.] is available
Wait done - object [Phonebook.] is available

[root@ambiorix]:~/.../subscriptions/labs/lab1$ 
[root@ambiorix]:~/.../subscriptions/labs/lab1$ ba-cli Phonebook.Contact.+ 
> Phonebook.Contact.+
Phonebook.Contact.1.

[root@ambiorix]:~/.../subscriptions/labs/lab1$ 
[root@ambiorix]:~/.../subscriptions/labs/lab1$ ba-cli Phonebook.Contact.+{FirstName="John"} 
> Phonebook.Contact.+{FirstName=John}
Phonebook.Contact.2.

[root@ambiorix]:~/.../subscriptions/labs/lab1$ 
[root@ambiorix]:~/.../subscriptions/labs/lab1$ 
[root@ambiorix]:~/.../subscriptions/labs/lab1$ ba-cli Phonebook.Contact.+{FirstName="Ella"}   <<<< Event received
> Phonebook.Contact.+{FirstName=Ella}
Phonebook.Contact.3.

Notification received from [Phonebook]:
{
    eobject = "Phonebook.Contact.",
    index = 3,
    keys = {
    },
    name = "3",
    notification = "dm:instance-added",
    object = "Phonebook.Contact.",
    parameters = {
        FirstName = "Ella",
        LastName = ""
    },
    path = "Phonebook.Contact."
}
[root@ambiorix]:~/.../subscriptions/labs/lab1$ 
[root@ambiorix]:~/.../subscriptions/labs/lab1$ 
[root@ambiorix]:~/.../subscriptions/labs/lab1$ 
[root@ambiorix]:~/.../subscriptions/labs/lab1$ ba-cli Phonebook.Contact.2.-
> Phonebook.Contact.2.-
Phonebook.Contact.2.
Phonebook.Contact.2.PhoneNumber.
Phonebook.Contact.2.E-Mail.

[root@ambiorix]:~/.../subscriptions/labs/lab1$ 
[root@ambiorix]:~/.../subscriptions/labs/lab1$ 
[root@ambiorix]:~/.../subscriptions/labs/lab1$ ba-cli Phonebook.Contact.3.-    <<<< Event received
> Phonebook.Contact.3.-
Phonebook.Contact.3.
Phonebook.Contact.3.PhoneNumber.
Phonebook.Contact.3.E-Mail.

Notification received from [Phonebook]:
{
    eobject = "Phonebook.Contact.",
    index = 3,
    keys = {
    },
    name = "3",
    notification = "dm:instance-removed",
    object = "Phonebook.Contact.",
    parameters = {
        FirstName = "Ella",
        LastName = ""
    },
    path = "Phonebook.Contact."
}
```


# Object Definition Language

## Introduction

The Object Definition Language provides a simple mean to define a data model.

A data model is a hierarchical tree of objects where each object can contain 0, 1 or more parameters and 0, 1 or more functions.
## Syntax Notation

Throughout this document odl syntax is provided in railroad [syntax diagrams](https://en.wikipedia.org/wiki/Syntax_diagram). In these diagrams you start reading from left to right and just follow the "rails".

Blocks with rounded corners are called "terminals", what is in that block just needs to be copied, except when it starts with `<` and ends with `>` . Blocks with straight corners are called "non-terminals" (note that the names of these are always in capitals), there will be another diagram for that block.

In some "terminals" the content in the block is between `<` and `>` characters. Read the correct topic in [Appendix A - Syntax Overview](#appendix-a-syntax-overview) for more information about these blocks.

White spaces must be put between each block, except when the block only contains a single symbol like `%`, `!`, `&`, `#`, `?`.

### Example Of Railroad Diagram

![[Pasted image 20240716054357.png]]

According to this diagram all of the following are valid `include` syntaxes:
```
include "my-file.odl";
include 'my-file.odl';
#include 'my-file.odl';
&include "my-file.odl";
?include 'my-file.odl':"backup-file.odl";
```
## Comments

In an odl file it is possible to write comments.
- Single line comments start with `//` and span till the end of the line (newline)
- Multi-line comments start with `/*` and end with `*/`

Comments can be used at any place in the odl file.
## Using Configuration Options and Environment Variables.

In an odl file it is allowed to use the defined config options at many places. Instead of using a fixed string, it can be replaced by `${<NAME>}`, in this notation the name references a configuration option. If no configuration option exists with the given name it is replaced with an empty string.

The same can be done with **environment variables, using the notation `$(<NAME>)`**, in this notation the name references a environment variable. If no environment variable exists with the given name it is replaced with an empty string.

```
${var}    - Local Variable
$(envvar) - Environment variable
```

Configuration options or environment variables can be used within a string to make it partially configurable, e.g.: `${prefix_}LocalData`, when the configuration option `prefix_` is defined and has the value `X_RPL_COM_` the result string will be `X_PRPL_COM_LocalData`.
#### Example
```
%config {
    prefix_ = "X_PRPL_COM_";
    myinclude = "test.odl";
}
include "${myinclude}";
%define {
    object MQTT {
        object "${prefix_}Extra" {
        }
    }
}
```

In above example the file `test.odl` will be included. The object `MQTT.X_PRPL_COM_Extra` will be defined.

> **NOTE**
> Using undefined configuration options or environment variables in the odl file could lead to some failures during parsing of the odl file. Undefined configuration options or environment variables will be replaced with an empty string.
> 
> It is also recommended to put fields using configuration options or environment variables between quotes (single or double)

## ODL Files

ODL (Object Definition Language) files are files that can define a data model and bind function implementations to these definitions. These definitions make it easy to define [BBF TR181](https://usp-data-models.broadband-forum.org/tr-181-2-15-1-usp.html) like data models in just minutes. When using the ambiorix tools and libraries it can be registered to different bus systems or use different protocols to make the defined data model(s) accessible to other applications (on the same host or remote).

Ambiorix uses a [Bus Agnostic API](https://gitlab.com/prpl-foundation/components/ambiorix/libraries/libamxb) to make it possible to create services and applications on top of different bus systems and protocols. Besides the Bus Agnostic Library for each supported bus system or protocol a specific back-end must be available.

At the moment of writing the following bus systems and protocols can be used:

- [ubus](https://gitlab.com/prpl-foundation/components/ambiorix/modules/amxb_backends/amxb_ubus).
- [pcb](https://gitlab.com/prpl-foundation/components/ambiorix/modules/amxb_backends/amxb_pcb) (SoftAtHome proprietary bus system).
- [USP](https://gitlab.com/prpl-foundation/components/ambiorix/modules/amxb_backends/amxb_usp)/IMTP - [USP](https://gitlab.com/prpl-foundation/components/ambiorix/modules/amxb_backends/amxb_usp)/MQTT.
- [RBus](https://gitlab.com/prpl-foundation/components/ambiorix/modules/amxb_backends/amxb_rbus) (in development, check the gitlab repository for the current state).
- [DBus](https://gitlab.com/prpl-foundation/components/ambiorix/modules/amxb_backends/amxb_dbus) (in development, check the gitlab repository for the current state).
- [JSONRPC](https://gitlab.com/prpl-foundation/components/ambiorix/modules/amxb_backends/amxb_jsonrpc) (currently not opensourced).

An ODL file is a structured text file. At the top level the following elements can be used:

- [include](#include)
- [import](#import)
- [requires](#requires)
- [config](#config)
- [define](#define)
- [populate](#populate)

![[Pasted image 20240716060506.png]]

# Include

With `include`, `#include`, `&include` or `?include` other odl files or directories containing odl files can be included. Parsing of the include file is done first - except when using `&include` - before continuing the current odl file (that contains the include).

Mandatory includes are specified with `include` or `&include`, optional includes with `#include`. When an optional include file or directory is not available, parsing of the current odl continues. When a mandatory include file or directory is not available, parsing stops with an error.

The conditional include `?include` takes two include files or directories separated with a `:`. When the first include file or directory is not found, the second file or directory will be loaded. If none of the files or directories exists, parsing stops with an error. If the first file is found, but is not a valid odl file, parsing stops with an error. If the first is a directory and contains an invalid odl file, parsing stops with an error.

When using `&include`, a check is done to verify that the file or directory exists, if the file or directory is not found, parsing stops with an error, otherwise the file or directory is added to a list of include files that need to be loaded. The file or directory included with `&include` will not be loaded immediately.

When post includes `&include` are loaded is depending on the application. When using the ambiorix run-time (`amxrt`) all post include files or directory (`&include`) are parsed after the `entry-points` with reason 0 (AMXO_START) are invoked. When the `entry-points` are called and all successful, all post include files and directories are loaded. After the post include files are loaded, the `entry-points` are called with reason 2 (AMXO_ODL_LOADED). This is mainly used when some initialization needs to be done before loading the default values. Typically a post include only contains a `%populate` section.

Includes can be done anywhere at top level in the odl file, outside a section (`%config`, `%define`, `%populate`).

The name of the file or directory must be put between quotes (single or double) and can contain an absolute path or relative path. When a relative path is specified (not starting with `/`), the file or directory is searched in the include dirs. (by default the current working directory). The include directories can be configured in the config option `include-dirs`.

The name of the odl file or directory may be fully or partially replaced with a configuration option or an environment variable.

Each `include` statement must be terminated with a `;`.
#### Mandatory includes:
-  `include "file.odl"`
- `&include "file.odl"`
#### Optional include:
- `#include "file.odl"`
#### Conditional include:
- `?include "file1.odl:file2.odl"`

> **NOTE**
> Recursive includes are not allowed. If file `A` includes file `B`, file `B` includes file `C` and file `C` includes `A` then the last include is invalid as it creates a recursive include.
> When recursive includes are detected, parsing will stop with an error.

## Include Syntax

![[Pasted image 20240716062308.png]]

## Include Example

```
%config {
    name = "tr181-mqtt";
}
#include "mod_sahtrace.odl";
#include "${name}_extra.odl";
include "${name}_definition.odl";
?include "${name}-save.odl:${name}-defaults.odl";
```

# Import

Throughout an odl file references to functions can be provided. The odl parser will try to resolve these function names to a function implementation. The parser gets help of function resolvers. Extra function resolvers can be registered to the odl parser. The odl parser already has 3 function resolvers, `auto` resolver (the default), the `function table` (ftab) resolver and the `import` resolver. The last one uses loaded shared objects (loaded with [dlopen](https://man7.org/linux/man-pages/man3/dlopen.3.html)) and tries to resolve the function names using [dlsym](https://man7.org/linux/man-pages/man3/dlsym.3.html).

With the `import` shared objects can be loaded. The loaded shared objects will be used by the `import resolver` to resolve function symbols in the loaded shared objects. An absolute or relative path may be provided together with the name of the shared object. When no path or a relative path is specified the shared object is searched in the import directories as specified by the `import-dirs` configuration section.

The `import` can be used anywhere at top level, between any sections. Make sure that shared objects are imported before using symbols from the shared object.

An alias for a shared object can be provided using `as "<NAME>"`

The `import` uses [`dlopen`](https://www.man7.org/linux/man-pages/man3/dlopen.3.html) to load the shared object, some of the attributes of `dlopen` can be specified:

- `RTLD_NOW` (from the linux man pages)

    > If this value is specified, or the environment variable LD_BIND_NOW is set to a nonempty string, all undefined symbols in the shared object are resolved before dlopen() returns. If this cannot be done, an error is returned.

- `RTLD_GLOBAL` (from the linux man pages)

    > The symbols defined by this shared object will be made available for symbol resolution of subsequently loaded shared objects.

The default behavior, if no attributes are specified, is `RTLD_LAZY` (from the linux man pages)

> Perform lazy binding. Resolve symbols only as the code that references them is executed. If the symbol is never referenced, then it is never resolved. (Lazy binding is performed only for function references; references to variables are always immediately bound when the shared object is loaded.)

An extra attribute is defined `RTLD_NODELETE` which will not unload the loaded shared object when no symbols are used.

> NOTE
> 
> - Shared objects of which no symbols are used, are unloaded after parsing the odl files, unless the flag `RTLD_NODELETE` is set on the import instruction.
> - Importing the same shared object multiple times (with different attributes), will have no effect. A shared object will be loaded only one time, all other imports of the same shared object are ignored.

An `alias` can be specified to make it easier to refer to the shared object. The `alias` can be used when defining the [entry-points](#define-entry-points) or can be used in [resolver instructions](#resolver-instructions).

The attributes are optional. Zero one or more of these attributes can be specified with the `import`.

The `import` must be terminated with a `;`.

Instead of specifying the shared object file name, the name may be replaced with a configuration option or an environment variable.

## Import Syntax

![[Pasted image 20240716063515.png]]
## Import Example
```
import "greeter_plugin.so" as "greeter";

%config {
    name = "tr181-mqtt"; 
}

import "${name}.so" as "${name}";
```
## Print

Using the `print` instruction messages can be printed to `stdout`. The print instruction can be used anywhere at top level, between sections.

It can be used to print the values of configuration options, which can be helpful in finding out what the current value of a configuration options is at that moment.

The `print` instruction must be terminated with a `;`.
### Print Syntax

![[Pasted image 20240716063817.png]]
### Print Example

```
%config{
    Option1 = "Value1";
}

print "Option1 = ${Option1}";
```

# Requires

Using the `requires` keyword a dependency to another part of the data model can be specified. The parser will not check if the required object is available, the object path will be put in a list of required object paths.

It is up to the application that uses the odl parser to check if all required objects are available and wait for them if needed.

When using the ambiorix runtime application (`amxrt`), it will check if the required objects are available and wait for them if they are not available. The ambiorix run time will register (build the loaded data model) only when all required objects are available, amxrt will only call the defined entry-points when all required objects are available.

The `requires` instruction must be terminated with a `;`.

Only one object path can be specified with the `requires` instruction, multiple `requires` instructions may be used.

### Requires Syntax

![[Pasted image 20240716064100.png]]

### Requires Example

```
requires "NetModel.Intf.";
```

# Sections

There are 3 kinds of sections available in an odl file:

- `%config` - see [Section %config](#section-%25config)
- `%define` - see [Section %define](#section-%25define)
- `%populate`

Section rules:

- All sections are optional, so an odl file without any section is valid.
- Each section can be used multiple times in a single odl file.
- The different kind of sections can appear in any order.

> **NOTE:**
> - Although the order of the sections doesn't matter, it matters that objects and parameters are defined in a `%define` section, before they are used in the `%populate` section.
> - When using configuration options in `<NAME>` or `<TEXT>` also make sure they are defined before using them.

Typically the first section used is the `%config` section.

## Section %config

In the config section values for configuration options can be specified. There is no restriction on which configuration options are set. Which configuration options will be used all depends on the application or plug-in.

Each config section starts with `%config {` and ends with `}`.

Everything between the curly braces `{` and `}` is considered as the body of the config section.

Each `%config` section has a scope, the values of the `options` defined in a `%config` section are only kept for that scope.

The scope of a `%config` section starts where it is defined and ends at the end of the file where it was defined. When an `include` is encountered the current configuration is passed to the include file.

In other words, changes in the configuration options are only visible in the current file, and in its included files but never in `parent` files (an odl file that included this one).

If a configuration option must be passed to the top level the attribute `%global` can be put before the name of the option. This will make the new value globally visible.

Each configuration option must be terminated with `;`.

#### Example of configuration option scope
Assume these two odl files, named `main.odl` and `included.odl`

**main.odl**:
```
print "Start - Option1 = ${Option1}";
%config{
    Option1 = "Value1";
}
print "Before include - Option1 = ${Option1}";
include "included.odl";
print "After include - Option1 = ${Option1}";
```

**included.odl**:
```
print "In included - Option1 = ${Option1}";
%config {
    Option1 = "ChangeValue";
}
print "End of included - Option1 = ${Option1}";
```

When parsing the `main.odl` file using an application that uses the odl parser you should get this output (if printing to stdout is available), here `amxrt` (AMbioriX RunTime) is used to parse the odl file.

```
$ amxrt main.odl
Start - Option1 =  (/home/sah4009/development/experiments/test_odls/main.odl@1)
Before include - Option1 = Value1 (/home/sah4009/development/experiments/test_odls/main.odl@7)
In included - Option1 = Value1 (/home/sah4009/development/experiments/test_odls/included.odl@1)
End of included - Option1 = ChangeValue (/home/sah4009/development/experiments/test_odls/included.odl@7)
After include - Option1 = Value1 (/home/sah4009/development/experiments/test_odls/main.odl@11)
```

Just before the end of `included.odl` the value of `Option1` is printed and is at that moment `ChangeValue`. When back in the `main.odl` after the include of `included.odl` the value of `Option` is back to the same value as before the include.

### Configuration Option Names

The names of configuration options can contain any character except space characters, but it is recommended to only use 0 - 9, a - z, A - Z, _ and -. When using other characters it is recommended to put the name between quotes (single or double), to avoid conflicts or misinterpretation by the parser.

If a name contains dots (`.`), it refers to an element which is in a table or array. If the dots are part of the name it must be put between double and single quotes.

A configuration option name can not use references to other configuration options or environment variables.

**Examples of configuration option names:**

```
%config {
    Option1 = "MyValue";
    "$Option2" = "MyValue";
    "Option3.Sub1" = "MyValue";
    "Option3.Sub2" = "MyValue";
    "'Option.With.Dots'" = "MyValue";
}
```

The above example defines 4 configuration options at top level:

- `Option1`
- `$Option2`
- `Option3` - which is a table
- `Option.With.Dots`

The table `Option3` will have to sub-values `Sub1` and `Sub2`. An alternative way to declare the table `Option3` is:

```
%config {
   Option3 = {
       Sub1 = "MyValue",
       Sub2 = "MyValue"
   };
}
```

### Configuration Option Values

A value can be any of the values as defined in [`<VALUE>`](#value).

The values in a table or an array can be again one of these types. In other words it is possible to add a table in an array or define complex configuration options. In tables and arrays each individual value must be terminated with a `,`, except the last one which is terminated with `}` or `]`.

**NOTE:** 
When redefining a table or an array the full table or array is replaced.

**Consider this example:**
```
%config {
    MyTable = {
        MySubTable = {
            MyOption1 = "Value1",
            MyOption2 = "Value2"
        },
        MySubArray = [ 1,2,3 ]
    };
}
%config {
    MyTable = {
        MySubTable2 = {
            Option1 = true,
            Option2 = false
        },
    };
}
```

The second `MyTable` definition will override the first definition of `MyTable`. In other words the configuration option `MyTable.MySubTable` will not exist anymore, including all sub-values if any. Also the array `MySubArray` will not exist anymore.

It is possible to change or add values in tables or arrays, by using the dot notation in the name.

```
%config {
    MyTable.MySubTable.MyOption2 = "ChangeValue";
    MyTable.MySubArray.0 = 0;
    MyTable.MySubArray.2 = 2;
    MyTable.MySubTable.MyOption3 = "NewValue";
}
```

### Extending Configuration Option Values

Config sections can contain list variables e.g.

```
%config {
    mylist = [ "foo" ];
}
```

It is possible to append items to the list by including an extra odl file.

For example:

```
%config {
    %global mylist += [ "bar"];
}
```

This results in `mylist` being `["foo", "bar"]`.

Besides lists, this can also be used for integers and strings. For example:

```
%config {
    mytext-config = "Hello";
    mynumber-config = 10;
}

%config {
    %global mytext-config += " World";
    %global mynumber-config += 20;
}

// Result mytext-config = "Hello World" 
//        mynumber-config = 30
```

### %config Syntax

![[Pasted image 20240716071607.png]]

**`<VALUE>`**

![[Pasted image 20240716071642.png]]

For more information about `<NAME>` read [<`NAME`>](#name) in [Appendix A - Syntax Overview](#appendix-a-syntax-overview).  
For more information about `<PATH>` read [<`PATH`>](#path) in [Appendix A - Syntax Overview](#appendix-a-syntax-overview).  
For more information about `<VALUE>` read [<`VALUE`>](#value) in [Appendix A - Syntax Overview](#appendix-a-syntax-overview).  
For more information about `<TEXT>` read [<`TEXT`>](#text) in [Appendix A - Syntax Overview](#appendix-a-syntax-overview).  
For more information about `<NUMBER>` read [<`NUMBER`>](#number) in [Appendix A - Syntax Overview](#appendix-a-syntax-overview).  
For more information about `<DATETIME>` read [<`DATETIME`>](#datetime) in [Appendix A - Syntax Overview](#appendix-a-syntax-overview).

### %config Example
**Example:**
```
%config {
    import-dbg = true;
    message = "My welcome message";
    %global auto-resolver-order = [ "import", "ftab", "*" ];
    personal-message = "${message} to everyone who reads this";
}
```

### ODL Parser Configuration Options

The parser itself uses the following configuration options:

- `include-dirs` - an array of include directories (quoted strings). Include files specified in the odl file with no path or a relative path are searched in these directories.
- `import-dirs` - an array of of import directories (quoted strings). Import files (plug-in shared objects) specified in the odl file with no path or a relative path are searched in these directories.
- `import-dbg` - boolean (true or false). Makes the import function resolver print more information to stderr.
- `silent` - boolean (true or false). When set to true no `import` errors or messages are printed, the `print` command that can be used between sections will also be silenced.
- `odl-import` - boolean (true or false). When set to true (when not defined, the default is true), the `import` will load the specified shared object file, when set to false no shared object specified with `import` are loaded.
- `auto-resolver-order` - an array of resolver names (quoted strings). The order specified in this list is the order the auto resolver invokes them to resolve a function symbols. This list can end with a `*` to indicate all other resolvers in no specified order. When the list is empty, the auto resolver will not resolve any symbol.
- `define-behavior` - is a key - value map, which can be used to change the behavior of the ODL parser when reading `%define` sections. The available keys are:
    - `existing-object` - possible values are "error" or "update", default behavior is "error"
    - `existing-parameter` - possible values are "error" or "update", default behavior is "error"
- `populate-behavior` - is a key - value map, which can be used to change the behavior of the ODL parser when reading `%populate` sections. The available keys are:
    - `unknown-parameter` - possible values are "error", "warning", "add", default behavior is "error". When set to "add" the parser will add the parameter to the object, even if it is not defined.
    - `duplicate-instance` - possible values are "error", "update", default behavior is "error". When set to "update" the parser will update the already added instance.
- `odl`
    - `buffer-size` - when saving in odl format (config or data model) a buffer is used to write in chunks of at least the specified buffer-size. When not defined a buffer size of 16K is used. The buffer is used to reduce the number of writes on the file system.

> **NOTE**
> 
> By default the parser will throw an error when defining multiple-times the same object or parameter. This behavior can be changed by setting `define-behavior.existing-object` and `define-behavior.existing-parameter` respectively to `update`. In that case it will update the object or parameter. It is possible to change the parameter `type` but it is not possible to change the object `type` (from singleton to multi-instance or the other way around).
> 
> By default the parser will throw an error when setting a parameter value in the `%populate` section for a non-defined parameter. This behavior can be changed by setting the `populate-behavior.unknown-parameter` to `add` which will then add the parameter using the value `type` as parameter type, no parameter attributes will be added, this behavior is not `recommended`. The setting can be changed to `warning` as well, the parser will then give a warning, but continues.
> 
> By default the parser will throw an error when creating a duplicate instance (same index, name or key values). This behavior can be changed by setting `populate-behavior.duplicate-instance` to `update`. In this case the parser will update the instance. Updating an instance can also be done by `selecting` the object and changing the `parameter` values.

## Section %define

In the `%define` section `mibs` (modular information blocks), the data model `objects` and `entry points` are defined. Objects can contain parameters, functions or other objects.

Each define section starts with `%define {` and ends with `}`.

Everything between the curly braces `{` and `}` is considered as the body of the define section.

In a `%define` section a hierarchical object tree can be defined. Such a hierarchical object tree is also known as a `data model`. It is possible to define a part of the TR181 data model as described in the [tr181 BBF data model](https://usp-data-models.broadband-forum.org/tr-181-2-15-1-usp.html).

Using `select` it is possible to extend already define objects.

> **NOTE**
> 
> MIBs are used to extend data model objects at run-time (or in the `%define` section), and are not compatible with definitions in [tr181 BBF data model](https://usp-data-models.broadband-forum.org/tr-181-2-15-1-usp.html). The data models defined by BBF are very static in nature, while the MIBs make the data model more dynamic, as extra parameters, objects and functions can be added and removed at any time.

An `%define` body may be empty.

### %define Syntax

![[Pasted image 20240716203706.png]]

### %define Example
```
%define {
}
```
