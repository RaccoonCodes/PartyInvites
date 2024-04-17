# Party Invites

This is one of the first Project written in ASP.NET Core MVC. his project is designed to manage guest responses for a party invitation. Users can submit their RSVP along with their name, email address, phone number, and whether they will attend the party or not.

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

** CHANGES IN PROGRESS
