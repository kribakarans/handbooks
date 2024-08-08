# AMXC Usage:

## Prerequisite:

Run Phonebook instance.

## Sample Data:

`rv = amxb_get(ctx, "Phonebook.Contact.", 0, data, 5);`

```
[
    {
        Phonebook.Contact. = {
        },
        Phonebook.Contact.1. = {
            FirstName = "John",
            LastName = "Doe"
        },
        Phonebook.Contact.2. = {
            FirstName = "Jane",
            LastName = "Doe"
        },
        Phonebook.Contact.3. = {
            FirstName = "Eva",
            LastName = "Elliott"
        }
    }
]
```

### Extract @list from the @data container

`list = GETP_ARG(&data, "0"); // Equivalent to amxc_var_get_path(&data, "0", AMXC_VAR_FLAG_DEFAULT);`

```
{
    Phonebook.Contact. = {
    },
    Phonebook.Contact.1. = {
        FirstName = "John",
        LastName = "Doe"
    },
    Phonebook.Contact.2. = {
        FirstName = "Jane",
        LastName = "Doe"
    },
    Phonebook.Contact.3. = {
        FirstName = "Eva",
        LastName = "Elliott"
    }
}
```

### Iterate each instances

```
amxc_var_for_each(instance, list) {
    kt_printn("Instance[%d]: '%s'\n", i++, amxc_var_key(instance));
    amxc_var_dump(instance, STDOUT_FILENO);
    kt_printf("    FirstName='%s'", GETP_CHAR(instance, "FirstName"));
    kt_printf("    LastName='%s'", GETP_CHAR(instance, "LastName"));
}
```

### First Instance:

```
element = amxc_var_get_first(list);
```

```
{
    FirstName = "John",
    LastName = "Doe"
}
```

### Next Instance:

```
element = amxc_var_get_next(element);
```

```
{
    FirstName = "Jane",
    LastName = "Doe"
}
```

### Last Instance:

```
element = amxc_var_get_last(list);
```

```
{
}
```

### Get instance from container object

Assumed container object that is enclosed with `[]`.

```
[
    {
        Phonebook.Contact. = {
        },
        Phonebook.Contact.1. = {
            FirstName = "John",
            LastName = "Doe"
        },
        Phonebook.Contact.2. = {
            FirstName = "Jane",
            LastName = "Doe"
        },
        Phonebook.Contact.3. = {
            FirstName = "Eva",
            LastName = "Elliott"
        }
    }
]
```

#### Get 0th instance from @data:

```
element = GETP_ARG(&data, "0.0");
```

```
{
    FirstName = "John",
    LastName = "Doe"
}
```

#### Get 1st instance from @data:

```
element = GETP_ARG(&data, "0.1");
```

```
{
    FirstName = "Jane",
    LastName = "Doe"
}
```

#### Get 2nd instance from @data:

```
element = GETP_ARG(&data, "0.2");
```

```
{
    FirstName = "Eva",
    LastName = "Elliott"
}
```

#### Get FirstName of 2nd instance from @data:

```
element = GETP_ARG(&data, "0.2.FirstName"); //amxc_var_get_path(&data, "0.2.FirstName", AMXC_VAR_FLAG_DEFAULT);
```

```
"Eva"
```

### Get instance from list object

Assumed list object that is enclosed with `{}`.

```
{
    Phonebook.Contact. = {
    },
    Phonebook.Contact.1. = {
        FirstName = "John",
        LastName = "Doe"
    },
    Phonebook.Contact.2. = {
        FirstName = "Jane",
        LastName = "Doe"
    },
    Phonebook.Contact.3. = {
        FirstName = "Eva",
        LastName = "Elliott"
    }
}
```

#### Get 0th instance from @list:

```
element = GETP_ARG(list, "0"); //amxc_var_get_path(list, "0", AMXC_VAR_FLAG_DEFAULT);
```

```
{
    FirstName = "John",
    LastName = "Doe"
}
```
#### Get 1st instance from @list:

```
element = GETP_ARG(list, "1"); //amxc_var_get_path(list, "1", AMXC_VAR_FLAG_DEFAULT);
```

```
{
    FirstName = "Jane",
    LastName = "Doe"
}
```
#### Get 2nd instance from @list:

```
element = GETP_ARG(list, "2"); //amxc_var_get_path(list, "2", AMXC_VAR_FLAG_DEFAULT);
```

```
{
    FirstName = "Eva",
    LastName = "Elliott"
}
```

#### Get LastName of 2nd instance from @list

```
element = GETP_ARG(list, "2.LastName");
```

### Get instance with Index from the list

```
{
    Phonebook.Contact. = {
    },
    Phonebook.Contact.1. = {
        FirstName = "John",
        LastName = "Doe"
    },
    Phonebook.Contact.2. = {
        FirstName = "Jane",
        LastName = "Doe"
    },
    Phonebook.Contact.3. = {
        FirstName = "Eva",
        LastName = "Elliott"
    }
}
```
#### Get 0th element from @list:

```
element = GETI_ARG(list, 0); //amxc_var_get_index(list, 0, AMXC_VAR_FLAG_DEFAULT);
```

```
{
    FirstName = "John",
    LastName = "Doe"
}
```

#### Get 1st element from @list:

```
element = GETI_ARG(list, 1); //amxc_var_get_index(list, 1, AMXC_VAR_FLAG_DEFAULT);
```

```
{
    FirstName = "Jane",
    LastName = "Doe"
}
```
#### Get 2nd element from @list:

```
element = GETI_ARG(list, 2); //amxc_var_get_index(list, 2, AMXC_VAR_FLAG_DEFAULT);
```

```
{
    FirstName = "Eva",
    LastName = "Elliott"
}
```

### Get Key Value of an Instance

**Sample Instance:**

```
{
    FirstName = "John",
    LastName = "Doe"
}
```

`val = GETP_ARG(instance, "FirstName"); //amxc_var_get_key(instance, "FirstName", AMXC_VAR_FLAG_DEFAULT);`

```
"John"
```

### Get Parent Object of an Instance

**Sample Object:**

```
{
    Phonebook.Contact. = {
    },
    Phonebook.Contact.1. = {
        FirstName = "John",
        LastName = "Doe"
    },
    Phonebook.Contact.2. = {
        FirstName = "Jane",
        LastName = "Doe"
    },
    Phonebook.Contact.3. = {
        FirstName = "Eva",
        LastName = "Elliott"
    }
}
```

**Test Instance:**

```
{
    FirstName = "John",
    LastName = "Doe"
}
```

```
parent = amxc_var_get_parent(instance);
```

**Parent (Output):**

```
{
    Phonebook.Contact. = {
    },
    Phonebook.Contact.1. = {
        FirstName = "John",
        LastName = "Doe"
    },
    Phonebook.Contact.2. = {
        FirstName = "Jane",
        LastName = "Doe"
    },
    Phonebook.Contact.3. = {
        FirstName = "Eva",
        LastName = "Elliott"
    }
}
```
