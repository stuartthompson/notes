# Vector Matching Macro

This macro has four matches:
* Zero or more arrays of literals followed by zero or more literals.
* Zero or more literals followed by zero or more arrays of literals.
* Zero or more literal values.
* Zero or more arrays of literal values.

## Macro

This macro attempts to funnel all matches into the third case, which builds a vector of vectors of the groups of values that were provided.

### Case 1
The first case looks for zero or more arrays of literals followed by zero or more literal values.

### Case 2
The second case looks for the opposite of case 1, matching zero or more literal values followed by zero or more arrays of literal values.

### Case 3
The third case looks for zero or more arrays of literal values. This case constructs the final output.

### Case 4
The fourth case looks for zero or more literal values and constructs a vector from them to be matched by case 3.
```
macro_rules! match_vecs {
    ( $([ $($arr:literal),+ ]),+ , $($lit:literal),+ ) => {
        svec!($([$($arr),*]),*, [$( $lit ),*]);
    };
    ( $($lit:literal),*, $([ $($arr:literal),+ ]),+ ) => {
        svec!([$($lit),*], $([$($arr),*]),*);
    };
    ( $([$($value:expr),*]),+ ) => {
        let mut m: Vec<Vec<&str>> = Vec::new();
        $(
            let mut v: Vec<&str> = Vec::new();
            $( v.push($value); )*
            m.push(v);
        )*
        
        println!("vec of vecs: {:?}", m);
    };
    ( $($value:expr),+ ) => {
        {
            svec!([$($value),*]);
        }
    };
}
```

## Examples

Matches the fourth case of the macro and is sent back into the macro to be matched by the third case as an array.
```
match_vecs!("One", "Two");
```

Matches the third case as a single array of values.
```
match_vecs!(["One", "Two"]);
```

Matches the third case as a repetition of an array of values.
```
match_vecs!(["One", "Two"], ["Three", "Four"]);
```

Matches the second case as a literal followed by arrays of values.
```
match_vecs!("One", ["Two", "Three"], ["Four"]);
```

Matches the first case as an array of values followed by literals.
```
match_vecs!(["One"], "Two", "Three", "Four");
```