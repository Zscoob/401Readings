# 410 readings
class 27, core & react

## JWT 
JWT is an acronym for JSON Web Token. JWT is a way for securely transmitting information between parties as a JSON object. This information is verified and trusted because it’s digitally signed. JWT’s can be using a secret (with the HMAC algorithm) or public/private key pair using RSA/ECDSA.
JWT structure contains three parts,
   - Header
   - Payload
   - Signature

example as follows:
______________________________________________________________
          {
          “token”: “eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJhZG1pbiIsImZ1bGxOYW1lIjoiVmFpYmhhdiBCaGFwa2FyIiwicm9sZSI6IkFkbWluIiwianRpIjoiMTJjY2JmMWQtZDRhOS00ODUyLWE5YTgtMTRiY2Y3NzA0MmQ1IiwiZXhwIjoxNTg2NTAxNzkxLCJpc3MiOiJodHRwczovL2xvY2FsaG9zdDo0NDMzNi8iLCJhdWQiOiJodHRwczovL2xvY2FsaG9zdDo0NDMzNi8ifQ.XS3LBDSRMhcJmUi2itgBbPPhrdbX2cFgC7tZ7X_einM”,
          “userDetails”:{
          “userName”: “admin”,
          “fullName”: “Vaibhav Bhapkar”,
          “password”: “1234”,
          “userRole”: “Admin”
          }
______________________________________________________________

### Header
The header consists of two-part one is signing algorithm and the other one is the type of the token which is JWT.
ex:
______________________________________________________________
          {
          “alg”: “HMAC”,
          “typ”: “JWT”
          }
______________________________________________________________
### Payload
The payload contains the claims. Claims are statements about an entity (typically, the user) and additional data. Again the payload formed JSON is Base64Url encoded to form second part.

### Signature
For the signature created you need to use encoded header, encoded payload, and secret with the algorithm specified at the header.
ex:
______________________________________________________________
          HMAC(
          base64UrlEncode(header) + “.” +
          base64UrlEncode(payload),
          secret)
______________________________________________________________
Putting all of these three parts together form Json Web Token.

## Steps
1. Create a new project:
   - Visual Studio
   - Create New Project 
   - Asp.Net Core Web application 
   - Select Web Api Template 
   - Create.
2. Install required a package of JWT from NuGet Package:
  - Right-click on project 
  - Manage NuGet package 
  - On browse section search for (Microsoft.AspNetCore.Authentication.JwtBearer) 
  - install.
3. Update Startup.cs ConfigureService() to register JWT authentication scheme:
______________________________________________________________
          services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
          .AddJwtBearer(options =>
          {
          options.RequireHttpsMetadata = false;
          options.SaveToken = true;
          options.TokenValidationParameters = new TokenValidationParameters
          {
          ValidateIssuer = true,
          ValidateAudience = true,
          ValidateLifetime = true,
          ValidateIssuerSigningKey = true,
          ValidIssuer = Configuration[“Jwt:Issuer”],
          ValidAudience = Configuration[“Jwt:Audience”],
          IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(Configuration[“Jwt:SecretKey”])),
          ClockSkew = TimeSpan.Zero
          ;
          });
______________________________________________________________

  - Token validation parameters are described below,
    - ValidateIssuerSigningKey — Gets or sets a boolean that controls if validation of the SecurityKey that signed the securityToken is called.
    - ValidIssuer — Gets or sets a String that represents a valid issuer that will be used to check against the token’s issuer.
    - ValidateIssuer — Gets or sets a value indicating whether the Issuer should be validated. True means Yes validation required.
    - ValidAudience — Gets or sets a string that represents a valid audience that will be used to check against the token’s audience
    - ValidateAudience — Gets or sets a boolean to control if the audience will be validated during token validation.
    - ValidateLifetime — Gets or sets a boolean to control if the lifetime will be validated during token validation.
    - ssuerSigningKey is the public key used for validating incoming JWT tokens. By specifying a key here
4. A further step is to change in the appsettings.json with required details for JWT authentication scheme:
______________________________________________________________
          “Jwt”: {
          “SecretKey”: “VaibhavBhapkar”,
          “Issuer”: “https://localhost:44336/",
          “Audience”: “https://localhost:44336/"
          },
______________________________________________________________
5. The next step is to make an authentication service available to the application to do this we need a call app.UseAuthentication in Configure() method:
______________________________________________________________
          public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
          {
          if (env.IsDevelopment())
          {
          app.UseDeveloperExceptionPage();
          }
          app.UseHttpsRedirection();
          app.UseRouting();
          app.UseAuthentication();
          app.UseAuthorization();
          app.UseEndpoints(endpoints =>
          {
          endpoints.MapControllers();
          });
          }
______________________________________________________________
6. Update Startup.cs file ConfigureService() method to add authorization code:
______________________________________________________________
          services.AddAuthorization(config =>
          {
          config.AddPolicy(Policies.Admin, Policies.AdminPolicy());
          config.AddPolicy(Policies.User, Policies.UserPolicy());
          });
______________________________________________________________
For authorization we define policies class there we defined the policy defined according to roles as follows:
______________________________________________________________
          using Microsoft.AspNetCore.Authorization;
          using System;
          using System.Collections.Generic;
          using System.Linq;
          using System.Threading.Tasks;
          namespace JWTAuthenticationExample.Models
          {
          public class Policies
          {
          public const string Admin = “Admin”;
          public const string User = “User”;
          public static AuthorizationPolicy AdminPolicy()
          { 
          return new AuthorizationPolicyBuilder().RequireAuthenticatedUser().RequireRole(Admin).Build();
          }
          public static AuthorizationPolicy UserPolicy()
          {
          return new AuthorizationPolicyBuilder().RequireAuthenticatedUser().RequireRole(User).Build();
          }
          }
          }
______________________________________________________________
7. Generate JSON Web Token:
We have to create a new API controller called LoginController and defined Login() method inside it, this method is responsible for the generation of JWT. we need to mark this method as AllowAnonymous attribute to bypass the authentication this method expects the User model object with a username and password parameter.
We have to define one more method called AuthenticateUser() which will check for user exists or not and return User object with other details if exists. This AuthenticateUser() method called inside Login() method once the user is authenticated we have to define one more method called GenerateJWTToken() where we have created a JWT using the JwtSecurityToken class. we need to create an object of this class bypassing some of the parameters to constructor includes issuer, audience, claims, expires, and signing credentials.
Claims here are the data contained by token they are informed about the user which is used to authorize the access the resource. Claims are formed using key-value pairs here in our case we are storing full name and role using claims.
Finally, JwtSecurityTokenHandler.WriteToken method is used to generate the JWT. This method expects an object of the JwtSecurityToken class.