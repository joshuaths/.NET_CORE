# Module 1:Tag Helpers

#### *Modules goals*
-  What is a tag helper 
	-  Why do you need them 
- First party tag helpers
-  Third party tag helpers 
- Create custom tag helpers. [Example] (https://github.com/dpaquette/TagHelperSamples/tree/master/TagHelperSamples/src/TagHelperSamples.Markdown)

### Working with Tag helpers
- Create a new ASP.NET Core project using the "Web Application" template with "Individual User Accounts" selected.

  ![image](https://github.com/LadyNaggaga/ASP.NETCoreMVA/blob/master/Images/new-webapp-individual-accounts.PNG)

- Open the view `Views/Account/Register.cshtml`
- Look at the Tag Helpers being used in this view (they're colored purple and in bold, make sure you build the project) and play around with setting their attributes and exploring the IntelliSense offered for the different attribute types
- Run the application and see the HTML output by the Tag Helpers
- Look at the other views in `Views/Account/` folder to see how they use Tag Helpers
- Open the file `Views/Shared/_Layout.cshtml`
- Look at the Tag Helpers being used in the `<head>` element and at the end of the page to render CSS stylesheets and JavaScript files and compare it to the generated HTML output

## Create a custom Tag Helper
- Create a new class in the application you created above, `public class RepeatTagHelper : TagHelper` and resolve any required namespaces
- Add a property `public int Count { get; set; }`
- Add an override for the TagHelper::ProcessAsync() method which repeats the content via the `output` parameter in a loop for `Count` iterations, e.g.:
  
  ``` C#
    public async override Task ProcessAsync(TagHelperContext context, TagHelperOutput output)
    {
        for (int i = 0; i < Count; i++)
        {
            output.Content.AppendHtml(await output.GetChildContentAsync(useCachedResult: false));
        }
    }
  ```

- Open the `_ViewImports.cshtml` file and add line to register your Tag Helper: `@addTagHelper "*, WebApplication49"` (adjust WebApplication49 to your assembly name)
- Open `Views/Home/Index.cshtml` and use your Tag Helper, e.g.:

  ``` HTML
  <repeat count="5">
    <h1>I'll be repeated!! @DateTime.UtcNow.Ticks</h1>
  </repeat>
  ```
  
- Run the application again and ensure your Tag Helper is working
- Note that the Tag Helper is rendering itself as a `<repeat>` tag (see it in the rendered HTML). We'll fix that now so that only the contents are rendered.
- Open the `RepeatTagHelper` again and in the `ProcessAsync` method add a line to null out the tag name: `output.TagName = null;`
- Run the application again and see that the outer tag is no longer rendered

## Extra 
- Experiment with decorating your `RepeatTagHelper` class with the `[HtmlTargetElement()]` attribute to change which HTML tag and/or attribute names it will attach itself to 



