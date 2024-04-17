# Party Invites

This is one of the first Project written in ASP.NET Core MVC. his project is designed to manage guest responses for a party invitation. Users can submit their RSVP along with their name, email address, phone number, and whether they will attend the party or not.

# Table of Contents

1. [Getting Started](#getting-started)
2. [Models](#models)
    - [GuestResponse.cs](#guestresponsecs)
    - [Repository.cs](#repositorycs)
3. [Views](#views)
    - [Index.cshtml](#indexcshtml)
    - [RsvpForm.cshtml](#rsvpformcshtml)
    - [Thanks.cshtml](#thankscshtml)
    - [ListResponses.cshtml](#listresponsescshtml)
    - [_Layout.cshtml](#_layoutcshtml)
4. [Controllers](#controllers)
    - [HomeController](#homecontroller)
5. [Summary](#summary)


## Getting Started
To use this project, Clone the repository and run it in Visual Studio or your preferred development environment. Once running, users can access the party invites form to submit their RSVP.

## Models

### GuestResponse.cs
```csharp
    public class GuestResponse
    {
        [Required(ErrorMessage = "Please enter your name")]
        public string? Name { get; set; }

        [Required(ErrorMessage = "Please enter your email address")]
        [EmailAddress]
        public string? Email { get; set; }

        [Required(ErrorMessage = "Please enter your phone number")]
        public string? Phone { get; set; }

        [Required(ErrorMessage = "Please specify whether you'll attend")]
        public bool? WillAttend { get; set;}
    }
```
This class represents the data model for a guest's response to the party invitation. It includes properties for the guest's name, email address, phone number, and whether they will attend the party or not. Data annotations are used for validation to ensure that required fields are filled out and that email addresses are in the correct format.

### Repository.cs 
```csharp
public static class Repository
{
    private static List<GuestResponse> responses = new();
    public static IEnumerable<GuestResponse> Responses => responses;
    public static void AddResponse(GuestResponse response)
    {
        Console.WriteLine(response);
        responses.Add(response);
    }
}
```
The Repository class provides a static repository to store guest responses. It includes a static list of GuestResponse objects to hold the responses. The AddResponse method allows new guest responses to be added to the repository.


## Views
There are 5 Important razor views that I will showcase. Reminder that Bootstrap and CSS is used throught the project.

### Index.cshtml
```cshtml
<body>
    <div class="text-center m-2">
        <h3> We're going to have an exciting party!</h3>
        <h4>And YOU are invited!</h4>
        <a class="btn btn-primary" asp-action="RsvpForm">RSVP Now</a>
    </div>
</body>
```
- This is the home page for Party Invites. It Display a simple welcome message and provides a button for users to RSVP to the Party.

### RsvpForm.cshtml
```cshtml
<body>
    <h5 class="bg-primary text-white text-center m-2 p-2">RSVP</h5>
   
    <form asp-action="RsvpForm" method="post" class="m-2">
        <div asp-validation-summary="All"></div>
        <div class="form-group">
            <label asp-for="Name" class="form-label">Your name:</label>
            <input asp-for="Name" class="form-control" />
        </div>
        <div class="form-group">
            <label asp-for="Email" class="form-label">Your email:</label>
            <input asp-for="Email" class="form-control" />
        </div>
        <div class="form-group">
            <label asp-for="Phone" class="form-label">Your phone:</label>
            <input asp-for="Phone" class="form-control" />
        </div>
        <div class="form-group">
            <label asp-for="WillAttend" class="form-label">
                Will you attend?
            </label>
            <select asp-for="WillAttend" class="form-control">
                <option value="">Choose an option</option>
                <option value="true">Yes, I'll be there</option>
                <option value="false">No, I can't come</option>
            </select>
        </div>
        <button type="submit" class="btn btn-primary mt-3">Submit RSVP</button>
    </form>
</body>
```
- This view is used to capture guest responses to the party invitation. It contains a form where users can enter their name, email address, phone number, and indicate whether they will attend the party.

### Thanks.cshtml
This page shows a thank you page that, depending if the user will attend, will display different output
```cshtml
<body class="text-center">
    <div>
        <h1>Thank you, @Model?.Name!</h1>
        @if (Model?.WillAttend == true)
        {
            @:It's great that you're coming. The drinks are already in the fridge!
        }
        else
        {
            @:Sorry to hear that you can't make it, but thanks for letting us know.
        }
    </div>
    Click
    <a asp-action="ListResponses">here</a> to see who is coming.
</body>
```
- This View displays a thank you message to the guest who has submitted their RSVP. It also provides information about whether the guest is attending the party or not. Also, There is a link that takes you to another page where you can see who is attending via asp-action to "ListResponses"

### ListResponses.cshtml
```cshtml
<body>
    <div class="text-center p-2">
        <h2 class="text-center">
            Here is the list of people attending the party
        </h2>
        <table class="table table-bordered table-striped table-sm">
            <thead>
                <tr><th>Name</th><th>Email</th><th>Phone</th></tr>
            </thead>
            <tbody>
                @foreach (PartyInvites.Models.GuestResponse r in Model!)
                {
                    <tr>
                        <td>@r.Name</td>
                        <td>@r.Email</td>
                        <td>@r.Phone</td>
                    </tr>
                }
            </tbody>
        </table>
    </div>
</body>
```
- The ListResponses.cshtml view displays a list of guests who have confirmed their attendance to the party. It presents their name, email address, and phone number in a table form.

### _Layout.cshtml

#### Header
```cshtml
<header>
        <nav class="navbar navbar-expand-sm navbar-toggleable-sm navbar-light bg-white border-bottom box-shadow mb-3">
            <div class="container-fluid">
                <a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">PartyInvites</a>
                <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target=".navbar-collapse" aria-controls="navbarSupportedContent"
                        aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                </button>
                <div class="navbar-collapse collapse d-sm-inline-flex justify-content-between">
                    <ul class="navbar-nav flex-grow-1">
                        <li class="nav-item">
                            <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Index">Home</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </header>
```
- The <header> section contains the navigation bar for the website.
- The navigation bar is created using Bootstrap classes for styling and responsiveness.
- It includes a brand logo (navbar-brand) linking to the home page and navigation links (nav-link) for the home page and privacy page.

#### Container class
```cshtml
<div class="container">
        <main role="main" class="pb-3">
            @RenderBody()
        </main>
    </div>
```
- The "@RenderBody()" is a Razor syntax that renders the content of the views within the layout. It acts like a placeholder where the content of each view will be placed.

#### Scripts
```chtml
    <script src="~/lib/jquery/dist/jquery.min.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
    @await RenderSectionAsync("Scripts", required: false)

```
- The <script> tags include JavaScript files for jQuery, Bootstrap, and any custom scripts used in the application.
- asp-append-version="true" ensures that the browser requests the latest version of the script files when they are updated.
- @await RenderSectionAsync("Scripts", required: false) renders any additional script sections defined in each views. This allows for views to include additional scripts specific to their functionality.

## Controllers
### HomeController
```csharp
public class HomeController : Controller
{

    public IActionResult Index()
    {
        return View();
    }

    [HttpGet]
    public ViewResult RsvpForm()
    {
        return View();
    }

    [HttpPost]
    public ViewResult RsvpForm(GuestResponse guestResponse)
    {
        if (ModelState.IsValid)
        {
            Repository.AddResponse(guestResponse);
            return View("Thanks", guestResponse);
        }
        else
        {
            return View();
        }

    }
    public ViewResult ListResponses()
    {
        return View(Repository.Responses.Where(r => r.WillAttend == true));
    }
}
```

#### RsvpForm(GuestResponse guestResponse)
Validates the form data using ModelState. If the form data is valid, adds the guest response to the repository and redirects to the Thanks view.If the form data is invalid, re-renders the RsvpForm view with validation errors.

#### ListResponses()
Renders a list of guests who have confirmed their attendance.It also filters the responses to include only those who will attend the party and then returns the ListResponses view.

## Summary
This project is a simple ASP.NET Core MVC application designed to manage guest responses for a party invitation. Users can submit their RSVPs along with their name, email address, phone number, and whether they will attend the party or not. 

The project includes models for guest responses and a repository to store the responses. Views are provided for capturing RSVPs, displaying a thank you message, and listing attendees. Additionally, a layout file is used for consistent styling and navigation. The HomeController handles user requests, including displaying the home page, capturing RSVPs, and listing attendees. 

Overall, Party Invites is a simple project that shows what ASP.NET Core MVC is Capable of doing.

