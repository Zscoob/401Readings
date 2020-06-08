# Introduction to Identity on ASP.NET Core
401 readings class 26

## Overview
ASP.NET Core Identity is a membership system that adds login functionality to ASP.NET Core apps. Users can create an account with the login information stored in Identity or they can use an external login provider. Supported external login providers include Facebook, Google, Microsoft Account, and Twitter.

Identity can be configured using a SQL Server database to store user names, passwords, and profile data. Alternatively, another persistent store can be used, for example, Azure Table Storage.

## AddDefaultIdentity and AddIdentity
AddDefaultIdentity was introduced in ASP.NET Core 2.1. Calling AddDefaultIdentity is similar to calling the following:
  - AddIdentity
  - AddDefaultUI
  - AddDefaultTokenProviders

## Create a Web app with authentication
Create an ASP.NET Core Web Application project with Individual User Accounts.
  - Visual Studio
    - Select File > New > Project.
    - Select ASP.NET Core Web Application. 
    - Name the project WebApp1 to have the same namespace as the project download. Click OK.
    - Select an ASP.NET Core Web Application, then select Change Authentication.
    - Select Individual User Accounts and click OK.

  - .NET Core CLI
    - dotnet new webapp --auth Individual -o WebApp1

The generated project provides ASP.NET Core Identity as a Razor Class Library. The Identity Razor Class Library exposes endpoints with the Identity area. For example:
  - /Identity/Account/Login
  - /Identity/Account/Logout
  - /Identity/Account/Manage

### Apply Migrations
Apply the migrations to initialize the database.
Run the following command in the Package Manager Console (PMC):
  - Visual Studio
    - PowerShell
    - Update-Database
  
  - .NET Core CLI
    - dotnet ef database update

### Test Register and Login
Run the app and register a user. Depending on your screen size, you might need to select the navigation toggle button to see the Register and Login links.

### View the Identity database

  - Visual Studio
    - From the View menu, select SQL Server Object Explorer (SSOX).
    - Navigate to (localdb)MSSQLLocalDB(SQL Server 13). Right-click on dbo.AspNetUsers > View Data:

### Configure Identity services
Services are added in ConfigureServices. The typical pattern is to call all the Add{Service} methods, and then call all the services.Configure{Service} methods.
______________________________________________________________
          public void ConfigureServices(IServiceCollection services)
          {
              services.Configure<CookiePolicyOptions>(options =>
              {
                  options.CheckConsentNeeded = context => true;
                  options.MinimumSameSitePolicy = SameSiteMode.None;
              });

              services.AddDbContext<ApplicationDbContext>(options =>
                  options.UseSqlServer(
                      Configuration.GetConnectionString("DefaultConnection")));
              services.AddDefaultIdentity<IdentityUser>()
                  .AddDefaultUI(UIFramework.Bootstrap4)
                  .AddEntityFrameworkStores<ApplicationDbContext>();

              services.Configure<IdentityOptions>(options =>
              {
                  // Password settings.
                  options.Password.RequireDigit = true;
                  options.Password.RequireLowercase = true;
                  options.Password.RequireNonAlphanumeric = true;
                  options.Password.RequireUppercase = true;
                  options.Password.RequiredLength = 6;
                  options.Password.RequiredUniqueChars = 1;

                  // Lockout settings.
                  options.Lockout.DefaultLockoutTimeSpan = TimeSpan.FromMinutes(5);
                  options.Lockout.MaxFailedAccessAttempts = 5;
                  options.Lockout.AllowedForNewUsers = true;

                  // User settings.
                  options.User.AllowedUserNameCharacters =
                  "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+";
                  options.User.RequireUniqueEmail = false;
              });

              services.ConfigureApplicationCookie(options =>
              {
                  // Cookie settings
                  options.Cookie.HttpOnly = true;
                  options.ExpireTimeSpan = TimeSpan.FromMinutes(5);

                  options.LoginPath = "/Identity/Account/Login";
                  options.AccessDeniedPath = "/Identity/Account/AccessDenied";
                  options.SlidingExpiration = true;
              });

              services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
          }
______________________________________________________________

The preceding code configures Identity with default option values. Services are made available to the app through dependency injection.

Identity is enabled by calling UseAuthentication. UseAuthentication adds authentication middleware to the request pipeline.
______________________________________________________________
          public void Configure(IApplicationBuilder app, IHostingEnvironment env)
          {
              if (env.IsDevelopment())
              {
                  app.UseDeveloperExceptionPage();
                  app.UseDatabaseErrorPage();
              }
              else
              {
                  app.UseExceptionHandler("/Error");
                  app.UseHsts();
              }

              app.UseHttpsRedirection();
              app.UseStaticFiles();
              app.UseCookiePolicy();

              app.UseAuthentication();

              app.UseMvc();
          }
______________________________________________________________

## Scaffold Register, Login, and LogOut
  - Visual Studio
    - Add the Register, Login, and LogOut files.
  
  - .NET Core CLI
    - If you created the project with name WebApp1, run the following commands. Otherwise, use the correct namespace for the ApplicationDbContext:
      - dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
      - dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
    - PowerShell uses semicolon as a command separator. When using PowerShell, escape the semicolons in the file list or put the file list in double quotes, as the preceding example shows.

### Examine Register
When a user clicks the Register link, the RegisterModel.OnPostAsync action is invoked. The user is created by CreateAsync on the _userManager object:
______________________________________________________________
          public async Task<IActionResult> OnPostAsync(string returnUrl = null)
          {
              returnUrl = returnUrl ?? Url.Content("~/");
              if (ModelState.IsValid)
              {
                  var user = new IdentityUser { UserName = Input.Email, Email = Input.Email };
                  var result = await _userManager.CreateAsync(user, Input.Password);
                  if (result.Succeeded)
                  {
                     _logger.LogInformation("User created a new account with password.");

                      var code = await _userManager.GenerateEmailConfirmationTokenAsync(user);
                      var callbackUrl = Url.Page(
                          "/Account/ConfirmEmail",
                          pageHandler: null,
                          values: new { userId = user.Id, code = code },
                          protocol: Request.Scheme);

                      await _emailSender.SendEmailAsync(Input.Email, "Confirm your email",
                          $"Please confirm your account by <a href='{HtmlEncoder.Default.Encode(callbackUrl)}'>clicking here</a>.");

                      await _signInManager.SignInAsync(user, isPersistent: false);
                      return LocalRedirect(returnUrl);
                  }
                  foreach (var error in result.Errors)
                  {
                      ModelState.AddModelError(string.Empty, error.Description);
                  }
             }

              // If we got this far, something failed, redisplay form
              return Page();
          }
______________________________________________________________If the user was created successfully, the user is logged in by the call to _signInManager.SignInAsync.

## Log in
The Login form is displayed when:
  - The Log in link is selected.
  - A user attempts to access a restricted page that they aren't authorized to access or when they haven't been authenticated by the system.
When the form on the Login page is submitted, the OnPostAsync action is called. PasswordSignInAsync is called on the _signInManager object.
______________________________________________________________
          public async Task<IActionResult> OnPostAsync(string returnUrl = null)
          {
              returnUrl = returnUrl ?? Url.Content("~/");

              if (ModelState.IsValid)
              {
                  // This doesn't count login failures towards account lockout
                  // To enable password failures to trigger account lockout, 
                  // set lockoutOnFailure: true
                  var result = await _signInManager.PasswordSignInAsync(Input.Email, 
                      Input.Password, Input.RememberMe, lockoutOnFailure: true);
                  if (result.Succeeded)
                  {
                      _logger.LogInformation("User logged in.");
                      return LocalRedirect(returnUrl);
                  }
                  if (result.RequiresTwoFactor)
                  {
                      return RedirectToPage("./LoginWith2fa", new { ReturnUrl = returnUrl, RememberMe = Input.RememberMe });
                  }
                  if (result.IsLockedOut)
                  {
                      _logger.LogWarning("User account locked out.");
                      return RedirectToPage("./Lockout");
                  }
                  else
                  {
                      ModelState.AddModelError(string.Empty, "Invalid login attempt.");
                      return Page();
                  }
                }

              // If we got this far, something failed, redisplay form
              return Page();
          }
______________________________________________________________
### Log out
The Log out link invokes the LogoutModel.OnPost action.

______________________________________________________________
          using Microsoft.AspNetCore.Authorization;
          using Microsoft.AspNetCore.Identity;
          using Microsoft.AspNetCore.Mvc;
          using Microsoft.AspNetCore.Mvc.RazorPages;
          using Microsoft.Extensions.Logging;
          using System.Threading.Tasks;

          namespace WebApp1.Areas.Identity.Pages.Account
          {
              [AllowAnonymous]
              public class LogoutModel : PageModel
              {
                  private readonly SignInManager<IdentityUser> _signInManager;
                  private readonly ILogger<LogoutModel> _logger;

                  public LogoutModel(SignInManager<IdentityUser> signInManager, ILogger<LogoutModel> logger)
                  {
                      _signInManager = signInManager;
                      _logger = logger;
                  }

                  public void OnGet()
                  {
                  }

                  public async Task<IActionResult> OnPost(string returnUrl = null)
                  {
                      await _signInManager.SignOutAsync();
                      _logger.LogInformation("User logged out.");
                      if (returnUrl != null)
                      {
                          return LocalRedirect(returnUrl);
                      }
                      else
                      {
                          // This needs to be a redirect so that the browser performs a new
                          // request and the identity for the user gets updated.
                          return RedirectToPage();
                      }
                  }
              }
          }
______________________________________________________________
SignOutAsync clears the user's claims stored in a cookie.

Post is specified in the Pages/Shared/_LoginPartial.cshtml:













