# Introduction #

Converters is a powerful mechanism that frees the View Model from putting too much display logic. Most display logic is supposed to declare in XML markup while keeping View Model as a data-provider.

Following is the list of Built-in Converters in latest version of Android Binding:



# Logical #

## IF ##

# of Arguments: 3

Argument Types: Boolean, Object, Object

Output Type: Object

Syntax:
```
IF( TEST-CONDITION, RESULT-TRUE, RESULT-FALSE )
```

Effect:

Return `RESULT-TRUE` if satisfies the `TEST-CONDITION`, else return the `RESULT-FALSE`

## NOT ##

# of Arguments: 1

Argument Types: Boolean

Output Type: Boolean

Syntax:
```
IF( BOOL-ARGUMENT )
```

Effect:

Return `true` if `BOOL-ARGUMENT` is false, else return `false`

# String Manipulation #

## CONCAT ##

# of Arguments: any

Argument Types: CharSequence

Output Type: CharSequence

Syntax:
```
CONCAT(ARGUMENT1, ARGUMENT2... ARGUMENTn)
```

Effect:

Return concatenated of args 1 thru n

## HTML ##

# of Arguments: any

Argument Types: CharSequence

Output Type: CharSequence

Syntax:
```
HTML(ARGUMENT1, ARGUMENT2... ARGUMENTn)
```

Effect:

Same as CONCAT, except any html tags will be processed

# Adapter Related #

Adapter type converters are used as `itemSource` of lists. Please see the **MultipleAdapters** demo in the MarkupDemo for more examples.

## STITCH ##

# of Arguments: any

Argument Types: Adapter

Output Type: CombinedAdapter (extends Adapter)

Syntax:
```
STITCH( ADAPTER1, ADAPTER2 ... ADAPTERn)
```

Effect:

Stitch (combine) all the adapters provided to be one single adapter. Since itemSource for ListViews and similar accepts only one adapter, this acts as an wrapper for supplying multiple adapters to one list.

## ADAPTER ##

# of Arguments: 1

Argument Types: [DynamicObject](DynamicObject.md), which required following parameters:

  1. template (of layout\_id)
> 2. source
> 3. spinnerTemplate (optional)

Output Type: Adapter

Syntax:
```
ADAPTER( {template=@layout/LAYOUT, source=SOURCE, spinnerTemplate=@layout/SPINNER_LAYOUT })
```

Effect:

Works when chained with other Adapter related Converters. An Adapter is created and supplied with the above mentioned arguments.

## SECTION ##

# of Arguments: 2

Argument Types: CharSequence, Layout Id
Output Type: SingletonAdapter (extends Adapter)

Syntax:
```
SECTION( SECTION_NAME, @layout/LAYOUT )
```

Effect:

SECTION results an adapter consists of only one element (thus, it is a _Singleton_). The `LAYOUT` provided will tell how to display the `SECTION_NAME`. In most cases, this works with STITCH converter.

# Others #

## ARGB ##

# of Arguments: 3 or 4

Argument Types: Integer

Output Type: Integer (Android Color int)

Syntax:
```
ARGB( Rvalue, Gvalue, Bvalue)
or
ARGB( Avalue, Rvalue, Gvalue, Bvalue)
```

Effect:

Convert the 3 or 4 components of color into Android Color integer