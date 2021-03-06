#Attributes of Layout and Profile

Here we discuss the important attributes of the `layout` and `profile` properties of [View](api:view). For a complete list, please refer to [LayoutDeclaration](api:view) and [ProfileDeclaration](api:view), respectively.

##The layout property

###type

Syntax: `type: none | linear | stack | tiles | table`  
Default: an empty string (i.e., `none`)

It specifies the type of the layout to use. It must be one of layouts managed by [LayoutManager](api:layout).

If not specified (i.e., `none`), the position of the child views won't be arranged. However, the dimensions (width and height) will, if you don't specify it explicitly.

> Currently, only none and linear are available

###width and height

Syntax: `width: #n | content | flex | flex #n | #n %`  
Syntax: `height: #n | content | flex | flex #n | #n %`

It specifies the default width (and height) for each child view. It is not the width (and height) for the view that `layout` is assigned to. To specify the width and height, use the `profile` property instead.

For more information, please refer to [the profile property](#profile_width) below.

###spacing

Syntax: `spacing: #n1 [#n2 [#n3 #n4]]`  
Default: 3

The spacing attribute specifies the spacing in pixel around the child view. It can be overridden by the child view's profile.

* If only a number is specified, it means all four spacing is the same as the given value.
* If two numbers are specified, the first number specifies the top and bottom spacing, while the second the left and right spacing.
* If all four numbers are specified, they specify the top, right, bottom, left spacing, respectively.

If the spacing at the left and at the right is different, the horizontal spacing of two adjacent views is the maximal value of them. Similarly, the vertical spacing is the maximal value of the spacing at the top and at the bottom.

![Spacing](spacing.jpg?raw=true)

If you prefer a different value, specify it in the gap attribute as described below.

###gap

Syntax: `gap: #n1 [#n2]`  
Default: *empty* (i.e., dependong on spacing)

The gap attribute specifies the spacing between two adjacent child views.

![Spacing with gap](spacing2.jpg?raw=true)

* If only a number is specified, it is used for the gap at both horizontal and vertical direction.
* If two numbers are specified, the first number specifies the gap at the vertical direction (top/bottom), and the second number the horizontal direction (left/right).

##The profile property

<a id="profile_width"></a>
##width and height

Syntax: `width: #n | content | flex | flex #n | #n %`  
Syntax: `height: #n | content | flex | flex #n | #n %`

It specifies the width (and height) of the given view.

    foo.profile.width = "100"; //100 pixel
    foo.profile.height = "content"; //best fit (the same size as the content)
    foo.profile.width = "20%"; //20% of the total available width
    foo.profile.height = "flex 3"; //see below

If the value is a percentage (such as 20%), it means the percentage of the total available space. For linear layout, it is the percentage of the parent's inner width (or height) (see also [View.innerWidth](api:view)).

If the value is `flex #n`, it means it will take all the remaining space. If two or more child views are specified with `flex #n`, the remaining space will be divided based on the number following `flex` (if no number, 1 is assumed).

For example, if a horizontal linear layout has three child views, their profile widths are `50`, `flex` and `flex 3`. If the inner width of the linear layout is 450. Then, their widths will be 50, 100 and 300, respectively.

    100 = (450 - 50) * 1 / (1 + 3);
    200 = (450 - 50) * 3 / (1 + 3);

Interesting to note that, if you specify the profile height to `flex` in a horizontal linear layout, it will be equivalent to `100%`, since there is only one child view in the vertical direction (for a horizontal linear layout).

##How to decide width and height

Here is the sequence the width of a view is decided (The decision sequence of height is the same). For sake of description, we call the view `foo`.

1. If `foo.profile.width` is specified, it is used
2. If both `foo.parent.layout.width` and `foo.parent.layout` are specified, `foo.parent.layout.width` is used.
3. If `foo.width` is specified, it is used.
4. If none of them is specified, it is assumed to be `content`.

Notice that it also means that the width (and height) set in the profile property has the higher priority than [View.width](api:view:set).
