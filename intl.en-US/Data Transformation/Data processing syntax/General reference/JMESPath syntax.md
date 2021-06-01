# JMESPath syntax

This topic describes the common JMESPath syntax.

JMESPath is an enhanced query and compute language for JSON. It can be used to extract, compute, and convert JSON data. For more information, see [JMESPath Tutorial](http://jmespath.org/tutorial.html).

The data transformation feature provides the `json_select`, `e_json`, and `e_split` functions. You can use JMESPath in these functions to extract values of fields or JSON expressions and compute specific values. Example:

```
json_select(Value, "JMESPath expression", ...)
e_json(Field, jmes="JMESPath expression", ...)
e_split(Field, ... jmes="JMESPath expression", ...)
```

For more information about how to use these functions, see [json\_select](/intl.en-US/Data Transformation/Data processing syntax/Expression functions/Structured data functions.md), [e\_json](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Value extraction functions.md), and [e\_split](/intl.en-US/Data Transformation/Data processing syntax/Global processing functions/Event processing functions.md).

## Extract a value by using a key

-   Raw log entry:

    ```
    "json_data":{
       "a": "foo",
       "b": "bar",
       "c": "baz"
     }
    ```

-   JMESPath syntax:

    ```
    json_select(v("json_data"), "a") # Return value: foo.
    json_select(v("json_data"), "b") # Return value: bar.
    json_select(v("json_data"), "c") # Return value: baz.
    ```


## Extract a nested value

-   Raw log entry:

    ```
    "json_data":{"a": 
                  {"b":
                    {"c":
                      {"d":"value"}
                  }
                }
             }
    ```

-   JMESPath syntax:

    ```
    json_select(v("json_data"), "a.b.c.d") # Return value: value.
    ```


## Extract values by using slicing data

-   Raw log entry:

    ```
    "json_data":{
       "a": ["b", "c", "d", "e", "f"]
     }
    ```

-   JMESPath syntax:

    ```
    json_select(v("json_data"), "a[2: ]") # Return value: ["d", "e", "f"].
    ```


## Extract a value by combining the preceding methods

-   Raw log entry:

    ```
    "json_data":{
      "a": {
        "b": {
          "c": [{"d": [0, [1, 2]]}, {"d": [3, 4]}]
        }
      }
    }
    ```

-   JMESPath syntax:

    ```
    json_select(v("json_data"), "a.b.c[0].d[1][0]") # Return value: 1.
    ```


## Extract values by using projection

-   Raw log entry 1:

    ```
    "json_data":{
        "people": [
          {"first": "James", "last": "d"},
          {"first": "Jacob", "last": "e"},
          {"first": "Jayden", "last": "f"},
          {"missing": "different"}
        ],
        "foo": {"bar": "baz"}
    }
    ```

-   JMESPath syntax 1:

    ```
    json_select(v("json_data"), "people[*].first") # Return value: ["James","Jacob","Jayden"].
    ```


-   Raw log entry 2:

    ```
    "json_data":{
        "ops": {
        "functionA": {"numArgs": 2},
        "functionB": {"numArgs": 3},
        "functionC": {"variadic": true}
        }
      }
    ```

-   JMESPath syntax 2:

    ```
    json_select(v("json_data"), "ops. *.numArgs") # Return value: [2, 3].
    ```


-   Raw log entry 3:

    ```
    "json_data":{
      "machines": [
        {"name": "a", "state": "running"},
        {"name": "b", "state": "stopped"},
        {"name": "c", "state": "running"}
      ]
    }
    ```

-   JMESPath syntax 3:

    ```
    json_select(v("json_data"), "machines[? state=='running'].name") # Return value: ["a", "c"].
    ```


## Extract values from a list

-   Raw log entry:

    ```
    "json_data":{
      "people": [
        {
        "name": "a",
        "state": {"name": "up"}
        },
        {
        "name": "b",
        "state": {"name": "down"}
        }
      ]
    }
    ```

-   JMESPath syntax:

    ```
    json_select(v("json_data"), "people[].[name, state.name]") # Return values: [["a","up"],["b","down"]]
    ```


## Compute the length of an array

-   Raw log entry:

    ```
    "json_data":{
       "a": ["b", "c", "d", "e", "f"]
     }
    ```

-   JMESPath syntax:

    ```
    json_select(v("json_data"), "length(a)") # Return value: 5.
    ```

    ```
    # Set the no-empty field to true if the length of array a is greater than 0.
    e_if(json_select(v("json_data"), "length(a)", default=0), e_set("no-empty", true))
    ```


