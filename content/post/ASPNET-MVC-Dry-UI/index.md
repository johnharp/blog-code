---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "ASP.NET MVC Dry UI"
subtitle: ""
summary: ""
authors: []
tags: ["ASP.NET MVC", "Razor"]
categories: []
date: 2020-06-30T08:13:09-05:00
lastmod: 2020-06-30T08:13:09-05:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

DRY (Don't Repeat Yourself) is a useful mantra on the way to 
maintainable, readable code, and ASP.NET MVC (with Razor view-engine)
gives you multiple ways to remove unnecessary redundancy in your views.


## @helper Methods

The @helper method syntax gives you the ability to package up a small bit of
markup and embeded C# server-side code in a reusable function that can be
called from the view.  When embedded in the view page itself it also has
access to any of the page's variables (including the Model).

Consider a view that contains four buttons, each of which has:
* A title
* Some explanatory text
* An icon
* A CSS class to apply colors
* An MVC action that should be called when clicked 

The desired output looks like this:
![Example with three buttons.](ex01.png "Example 1")

And here's the markup to create it:

##### HelperInPage.cshtml

 {{< highlight html "linenos=table,linenostart=13" >}}
<a href='@Url.Action("Action1")'
   class="buttonex buttonex-1">
    <div class="text-left">
        <i class="mdi mdi-comment-text"></i>
        <b>Button 1</b>
        <p class="mb-0">Description of first button</p>
    </div>
</a>

<a href='@Url.Action("Action2")'
   class="buttonex buttonex-2">
    <div class="text-left">
        <i class="mdi mdi-email-plus-outline"></i>
        <b>Button 2</b>
        <p class="mb-0">Description of second button</p>
    </div>
</a>


<a href='@Url.Action("Action3")'
   class="buttonex buttonex-3">
    <div class="text-left">
        <i class="mdi mdi-hexagon-slice-4"></i>
        <b>Button 3</b>
        <p class="mb-0">Description of third button</p>
    </div>
</a>
{{< /highlight >}}

It's pretty clear there is a lot of repetition going on here.

At the bottom of the view we can make use of the @helper syntax to
create a re-usable function:

{{< highlight html "linenos=table,linenostart=58" >}}
@helper ActionButton(
    string title, string description,
    string buttonCssClass, string iconCssClass,
    string action)
{
    <a href='@Url.Action(action)'
       class="buttonex @buttonCssClass">
        <div class="text-left">
            <i class="mdi @iconCssClass"></i>
            <b>@title</b>
            <p class="mb-0">@description</p>
        </div>
    </a>
}
{{< /highlight >}}

Which simplifies our buttons down to this:
{{< highlight html "linenos=table,linenostart=46" >}}
    @ActionButton("Button 1", "Description of first button", "buttonex-1", "mdi-comment-text", "Action1")
    @ActionButton("Button 2", "Description of second button", "buttonex-2", "mdi-email-plus-outline", "Action2")
    @ActionButton("Button 3", "Description of third button", "buttonex-3", "mdi-hexagon-slice-4", "Action3")
{{< /highlight >}}

## @helper Methods -- Shared in App_Code

Helper methods don't only have to live in a single view page.
If you have a snippet that should be shared 
you can store this helper method definition in a Razor view page under
App_Code.

* Create a directory "App_Code" directly under the solution root
  (if it doesn't already exist)
* Create a file "MyHelpers.cshtml" for example.

![Adding helpers to App_Code folder.](appcode-helper-vs.png "App_Code Helpers")

Move our ActionButton helper to the MyHelpers.cshtml page.


