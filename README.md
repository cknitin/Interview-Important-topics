# Interview-Important-topics
Important topics for interview in MVC, WebAPI, C#, Azure SR

## 1.       MVC

  - a.       MVC lifecycle
  - b.       Routing
  - c.       Views
  - d.       Razor Views
  - e.       Filters
  - f.       Authentication
  - g.       Security implementation
  - h.       Exception handling
             
    - 1. try/catch
             
                  public ActionResult Index()
                  {
                      try
                      {
                          int a = 1;
                          int b = 0;
                          int c = 0;
                          c = a / b; //it would cause exception. 

                          return View();
                      }
                      catch
                      {
                          Trace.Write("Error");

                          return View("Error");
                      }
                  }
                  
    - 2. In web.config
                  
                  <system.web>
                    <customErrors mode="On" defaultRedirect="~/ErrorHandler/Index">
                        <error statusCode="404" redirect="~/ErrorHandler/NotFound"/>
                    </customErrors>
                  </system.web>
                               
    - 3. using [HandleError] attribute
             
                  [HandleError(ExceptionType = typeof(DivideByZeroException), View = "~/Views/CommonExceptionView.cshtml")]
                  public ActionResult Contact()
                  {
                      int a = 1;
                      int b = 0;
                      int c = 0;
                      c = a / b; //it would cause exception.    
                      return View();
                  }
             
    - 4. Overriding OnException() method of controller base class
             
                  public class HomeController : Controller
                  {
                      public ActionResult Index()
                      {
                          return View();
                      }

                      protected override void OnException(ExceptionContext filterContext)
                      {
                          filterContext.ExceptionHandled = true;

                          //Log the error!!
                          Trace.Write(filterContext.Exception);

                          //Redirect or return a view, but not both.
                          filterContext.Result = RedirectToAction("Index", "ErrorHandler");
                          // OR 
                          filterContext.Result = new ViewResult
                          {
                              ViewName = "~/Views/ErrorHandler/Index.cshtml"
                          };
                      }
                  }
                  
    - 5. HandleErrorAttribute by create action filter class
                  
                  [CustomErrorHandling]
                  public ActionResult About()
                  {
                      int a = 1;
                      int b = 0;
                      int c = 0;
                      c = a / b; //it would cause exception.    

                      ViewBag.Message = "Your application description page.";

                      return View();
                  }
                  
                  public class CustomErrorHandling : HandleErrorAttribute
                  {
                      public override void OnException(ExceptionContext filterContext)
                      {
                          if (filterContext.ExceptionHandled || filterContext.HttpContext.IsCustomErrorEnabled)
                          {
                              return;
                          }
                          Exception e = filterContext.Exception;
                          filterContext.ExceptionHandled = true;
                          filterContext.Result = new ViewResult()
                          {
                              ViewName = "~/Views/CommonExceptionView.cshtml"
                          };
                      }

                  }

             
    - 6. Application_Error() using Global.asax page.
                  
                  protected void Application_Error()
                  {
                      var ex = Server.GetLastError();
                      //log the error!
                      Trace.Write(ex);
                  }
  
  - i.       Caching
  - j.       Validations
  - k.       Areas
  - l.       Cookies
  - m.       Value Provider / Custom Value Provider
  - n.       handler
  
   Create a class library project and impliment "IHttpHandler"    
            public class RssHandler : IHttpHandler
            {
                public bool IsReusable
                {
                    get
                    {
                        return false;
                    }
                }

                public void ProcessRequest(HttpContext context)
                {
                    context.Response.ContentType = "text/html";

                    using (XmlWriter writer = XmlWriter.Create(context.Response.OutputStream))
                    {
                        writer.WriteStartDocument();
                        writer.WriteElementString("rss", "This is test feed.");
                        writer.WriteEndDocument();
                        writer.Flush();
                    }
                }
            } 
            
 Add Class library project reference in MVC project. Also add the below line in web.config            
                
                <system.webServer>
                  <handlers>
                    <add name="RssHandler" verb="*" path="*.rss" type="CustomHttpHandler.RssHandler, CustomHttpHandler"/>
                  </handlers>
                </system.webServer>
  
  - o.       module
              
    Create a class library project and impliment "IHttpModule"            
              
              public class LogginHttpModule : IHttpModule
              {
                  public void Dispose()
                  {

                  }

                  public void Init(HttpApplication context)
                  {
                      context.LogRequest += Context_LogRequest;
                  }

                  private void Context_LogRequest(object sender, EventArgs e)
                  {
                      HttpApplication application = (HttpApplication)sender;
                      HttpContext context = application.Context;

                      string requestPath = context.Request.Path;

                      Trace.WriteLine(String.Format("Request Path: {0}",requestPath));

                  }
              } 
              
      Add Class library project reference in MVC project.
      Also add the below line in web.config        
              
              <system.webServer>
                <modules>
                  <add name="LogginHttpModule" type="CustomHttpModule.LogginHttpModule, CustomHttpModule"/>
                </modules>
              </system.webServer>
              
  - p. PreApplicationStartMethod          
  
  This method will call before the Application_Start() method.
  
        [assembly: PreApplicationStartMethod(typeof(PreApplicationStartup), "Initialize")]
        namespace PreApplicationStartMethodDemo
        {
            public class PreApplicationStartup
            {

                public static void Initialize()
                {

                }
            }
        }
        
  or in side the "AssemblyInfo.cs"
  
  [assembly: PreApplicationStartMethod(typeof(PreApplicationStartup), "Initialize")]
  
  Now add the project reference in MVC project

  

## 2.       Web API

  - a.       Authentication
  - b.       Authorization
  - c.       Routing
  - d.       Content Negotiation
  - e.       Security implementation
  - f.       Exception Handling
  - g.       Dependency injection
  - h.       Http message handlers
  - i.       HTTP Cookies

## 3.       C#

  - a.       Generics
  - b.       Delegates
  - c.       Lambda Expressions
  - d.       Events
  - e.       Extension Methods
  - f.       LINQ
  - g.       Nullable Types
  - h.       Dynamic
  - i.       Exception Handling
  - j.       Asynchronous / Parallel Programming
  - k.       Entity Framework

## 4.       Azure

  - a.       Azure logic apps
  - b.       Azure Service Bus
  - c.       Azure Functions
  - d.       Azure API Management
  - e.       Azure Web Apps/API apps
  - f.       Azure Resource manager scripts (aka AzureRM or ARM) Or Azure infrastructure provisioning using Terraform
  - g.       Azure storage

## 5.       Need to have good working experience in automating Unit Testing using nUnit.
## 6.       Need to have a good working experience in using source control management tools using Git/TFS etc.
## 7.       Good to have CI/CD experience with Teamcity/Jenkins, Octopus
## 8.       Need to have good working knowledge of design pattern and SOLID principles.
## 9.       Need to have Good implementation Knowledge in Oops Concepts.
## 10.      Need to have Excellent Communication Skills.
