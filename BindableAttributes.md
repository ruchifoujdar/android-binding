# Introduction #

This is a book-keeping post that keep the list of supporting attributes.

Remarks: non-optional attribute means if it's omitted, it will eventually breaks the function of that Widget, for example, if no itemTemplate for ListView, the ListView will simply render nothing even you provided the itemSource and everything else.

# Index #




# View #
| Attribute | Accepted Data type | Currently Supported |
|:----------|:-------------------|:--------------------|
| background | drawable, Resource Integer | v0.22               |
| backgroundColor | Color Integer      | v0.22               |
| onClick   | Command            | yes                 |
| onLongClick | Command            | yes                 |
| visibility | int, boolean       | yes                 |
| enabled   | boolean            | yes                 |

## View Groups ##
View groups include all layout objects (relative, linear etc).
No special attributes, inherited from view

### View Animator ###
| Attribute | Accepted Data type | Currently Supported |
|:----------|:-------------------|:--------------------|
| displayedChild | int                | 0.15                |
| childViews | View`[]` (Observable<View`[]`>) | 0.15                |

## Image View ##
| Attribute | Accepted Data type | Currently Supported |
|:----------|:-------------------|:--------------------|
| source    | drawable, Integer(Resource), Uri, Bitmap | yes                 |
| tint      | color integer      | no                  |

## Progress Bar ##
| Attribute | Accepted Data type | Currently Supported |
|:----------|:-------------------|:--------------------|
| progress  | float #            | yes                 |
| secondaryProgress | float #            | yes                 |

# remark: this is different from original, it is float and should be ranging from 0..1

### Seek Bar ###
| Attribute | Accepted Data type | Currently Supported |
|:----------|:-------------------|:--------------------|
| onSeekBarChange | Command            | v0.22               |

### Rating Bar ###
| Attribute | Accepted Data type | Currently Supported |
|:----------|:-------------------|:--------------------|
| rating    | float              | yes                 |
| onRatingChanged | Command            | yes                 |

## Text View ##
| Attribute | Accepted Data type | Currently Supported |
|:----------|:-------------------|:--------------------|
| lines     | int                | no                  |
| text      | CharSequence       | yes                 |
| textColor | int                | 0.22                |
| onTextChanged | Command            | yes                 |

### Compound Button ###
| Attribute | Accepted Data type | Currently Supported |
|:----------|:-------------------|:--------------------|
| onCheckedChange | Command            | yes                 |
| checked   | boolean            | yes                 |

## Adapter View ##
| Attribute | Accepted Data type | Currently Supported |
|:----------|:-------------------|:--------------------|
| onItemLongClicked | Command            | no                  |
| onItemSelected | Command            | yes                 |
| selectedPosition | Integer            | 0.31                |
| selectedItem | Object (readonly)  | yes                 |
| selectedId | long (readonly)    | yes                 |
| itemSource | Array, Cursor| experimental        |
| itemTemplate | Layout Resource Id | yes                 |
| adapter   | Adapter (overrides the itemSource/Template) | yes                 |
| headerItem | Object (view model) | no (schedule in 0.2) |
| headerItemTemplate | Layout Resource Id | no (schedule in 0.2) |
| footerItem | Object (view model) | no (schedule in 0.2) |
| footerItemTemplate | Layout Resource Id | no (schedule in 0.2) |

**Remark
For array type of Item Source, two way binding is supported. Current restriction: Array must consists of exactly same type of objects, the object can consists of Observable / Command fields, like what you define in models.**

For Cursor type, only one way is supported; You need to supply a defined CursorRowModel to pass to the Binder. (TODO: Document needed)
In such case, selectedItem / itemSource will return a copy of the CursorRowModel filled with the data from cursor. If the data is modified, a manual calling to refresh the cursor is required.

### List View ###
| Attribute | Accepted Data type | Currently Supported |
|:----------|:-------------------|:--------------------|
| onItemClicked | Command            | yes                 |
| clickedItem | Object (readonly)  | yes                 |
| clickedId | long (readonly)    | yes                 |

### Abs Spinner ###
| Attribute | Accepted Data type | Optional | Currently Supported |
|:----------|:-------------------|:---------|:--------------------|
| spinnerTemplate | Layout resource Id | yes (if unset, will use itemTemplate) | yes                 |
| selectedPosition | Integer            | yes      | 0.31                |

**Remark
The selectedPosition returns the position (i.e. index) of the selection.**



### ListView ###
| Attribute | Accepted Data type | Optional | Currently Supported |
|:----------|:-------------------|:---------|:--------------------|
| checkedItemPosition  | Integer            | yes      | 0.31                |
| checkedItemPositions  | SparseBooleanArray | yes      | 0.31                |

**Remark
Note the name of the above two attributes, only one (s) is different. First one only function when the `ChoiceMode` of the ListView is set to `singleChoice`, while the second one is set to `multipleChoice`. Due to performance consideration, only position of item(s) is returned, developers are responsible to determine which object(s) is returned.**

### Expandable List View ###
| Attribute | Accepted Data type | Optional | Currently Supported |
|:----------|:-------------------|:---------|:--------------------|
| childItemSource | String (Binding Syntax) | no       | yes                 |
| childItemTemplate | Layout resource Id | no       | yes                 |
| clickedChild | Object             | yes      | 0.3                 |
| onChildClick | Command            | yes      | 0.3                 |