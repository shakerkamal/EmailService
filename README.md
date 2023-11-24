# Email Sender Interface
> A class library to encapulate email sending process, that works like an adapter. Just plug and send emails.

## Features
 - Sending email to multiple users<br>
 - Sending email with attachments<br>
 - Sending email with any SMTP provider like - SendGrid, Mailchimp, Mailgun etc.

## Built with
> Dotnet 6.0

## Using the project

Please download the project and copy it to your existing solution. Add project reference to the dependent project. 

Initialize the `IEmailSender` interface and inject it in the constructor of the class. Suppose you have a dumy class `EmployeeRegistration`.
```
private readonly IEmailSender _emailSender;

public EmployeeRegistration(..., IEmailSender emailSender)
{
    _emailSender = emailSender;
}

public RegisterUser(...)
{
    /*
    Your code implementation goes here
    */
    var email = new Message
    {
        ToAddresses = new List<string> { user.Email },
        Subject = "Your subject",
        Body = message
    };
    _emailSender.SendEmailAsync(email);
}
```
For sending an email with attachment you just need to pass the attachment in the Message object.
```
public SendGreetings(...)
{
    /*
    Your code implementation goes here
    */
    var greetingsPdf = GeneratedGreetingPdf();
    using(var stream = new MemoryStream(greetingsPdf))
    {
        var file = new FormFile(stream, 0, report.Length, "Your Pdf Name", $"YourPdfName.pdf")
        {
            Headers = new HeaderDictionary(),
            ContentType = "application/octet-stream",
        };

        System.Net.Mime.ContentDisposition cd = new System.Net.Mime.ContentDisposition
            {
                FileName = file.FileName
            };
        file.ContentDisposition = cd.ToString();
        var files = new FormFileCollection();
        files.Add(file);
        var email = new Message
        {
            ToAddresses = new List<string> { user.Email },
            Subject = "Your subject",
            Body = message,
            Attachments = files
        };
        _emailSender.SendEmailAsync(email);
    }
}
```
