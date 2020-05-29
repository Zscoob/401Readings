# Creating Unit Tests for ASP.NET MVC Applications
401 readings class 20

## Creating the Controller under Test
The controller, named the ProductController.

________________________________________________________________________________________________
          using System;
          using System.Web.Mvc;

          namespace Store.Controllers
          {
               public class ProductController : Controller
               {
                    public ActionResult Index()
                    {
                         // Add action logic here
                         throw new NotImplementedException();
                    }

                    public ActionResult Details(int Id)
                    {

                         return View("Details");
                    }
               }
          }
________________________________________________________________________________________________
The ProductController contains two action methods named Index() and Details(). Both action methods return a view. Notice that the Details() action accepts a parameter named Id.

## Testing the View returned by a Controller
Imagine that we want to test whether or not the ProductController returns the right view. We want to make sure that when the ProductController.Details() action is invoked, the Details view is returned.
________________________________________________________________________________________________
          using System.Web.Mvc;
          using Microsoft.VisualStudio.TestTools.UnitTesting;
          using Store.Controllers;

          namespace StoreTests.Controllers
          {
               [TestClass]
               public class ProductControllerTest
               {
                    [TestMethod]
                    public void TestDetailsView()
                    {
                         var controller = new ProductController();
                         var result = controller.Details(2) as ViewResult;
                         Assert.AreEqual("Details", result.ViewName);

                    }
               }
          }
________________________________________________________________________________________________
The class in Listing 2 includes a test method named TestDetailsView(). This method contains three lines of code. The first line of code creates a new instance of the ProductController class. The second line of code invokes the controller's Details() action method. Finally, the last line of code checks whether or not the view returned by the Details() action is the Details view.

The ViewResult.ViewName property represents the name of the view returned by a controller. One big warning about testing this property. There are two ways that a controller can return a view. A controller can explicitly return a view like this:
________________________________________________________________________________________________
          public ActionResult Details(int Id)
          {
               return View("Details");
          }
________________________________________________________________________________________________
Alternatively, the name of the view can be inferred from the name of the controller action like this:
________________________________________________________________________________________________
          public ActionResult Details(int Id)
          {
               return View();
          }
________________________________________________________________________________________________
This controller action also returns a view named Details. However, the name of the view is inferred from the action name. If you want to test the view name, then you must explicitly return the view name from the controller action.

You can run the unit test in Listing 2 by either entering the keyboard combination Ctrl-R, A or by clicking the Run All Tests in Solution button 

## Testing the View Data returned by a Controller

An MVC controller passes data to a view by using something called View Data. For example, imagine that you want to display the details for a particular product when you invoke the ProductController Details() action. In that case, you can create an instance of a Product class (defined in your model) and pass the instance to the Details view by taking advantage of View Data.

The modified ProductController includes an updated Details() action that returns a Product.
________________________________________________________________________________________________
          using System;
          using System.Web.Mvc;

          namespace Store.Controllers
          {
               public class ProductController : Controller
               {
                    public ActionResult Index()
                    {
                         // Add action logic here
                         throw new NotImplementedException();
                    }

                    public ActionResult Details(int Id)
                    {
                         var product = new Product(Id, "Laptop");
                         return View("Details", product);
                    }
               }
          }
________________________________________________________________________________________________
First, the Details() action creates a new instance of the Product class that represents a laptop computer. Next, the instance of the Product class is passed as the second parameter to the View() method.

You can write unit tests to test whether the expected data is contained in view data. The unit test tests whether or not a Product representing a laptop computer is returned when you call the ProductController Details() action method.
________________________________________________________________________________________________
          using System.Web.Mvc;
          using Microsoft.VisualStudio.TestTools.UnitTesting;
          using Store.Controllers;

          namespace StoreTests.Controllers
          {
               [TestClass]
               public class ProductControllerTest
               {

                    [TestMethod]
                    public void TestDetailsViewData()
                    {
                         var controller = new ProductController();
                         var result = controller.Details(2) as ViewResult;
                         var product = (Product) result.ViewData.Model;
                         Assert.AreEqual("Laptop", product.Name);
                    }
               }
          }
________________________________________________________________________________________________
the TestDetailsView() method tests the View Data returned by invoking the Details() method. The ViewData is exposed as a property on the ViewResult returned by invoking the Details() method. The ViewData.Model property contains the product passed to the view. The test simply verifies that the product contained in the View Data has the name Laptop.

## Testing the Action Result returned by a Controller

A more complex controller action might return different types of action results depending on the values of the parameters passed to the controller action. A controller action can return a variety of types of action results including a ViewResult, RedirectToRouteResult, or JsonResult.

For example, the modified Details() action in Listing 5 returns the Details view when you pass a valid product Id to the action. If you pass an invalid product Id -- an Id with a value less than 1 -- then you are redirected to the Index() action.

________________________________________________________________________________________________
          using System;
          using System.Web.Mvc;
          namespace Store.Controllers
          {
               public class ProductController : Controller
               {
                    public ActionResult Index()
                    {
                         // Add action logic here
                         throw new NotImplementedException();
                    }
                    public ActionResult Details(int Id)
                    {
                         if (Id < 1)
                              return RedirectToAction("Index");
                         var product = new Product(Id, "Laptop");
                         return View("Details", product);

                    }
               }
          }
________________________________________________________________________________________________

You can test the behavior of the Details() action with the unit test in Listing 6. The unit test verifies that you are redirected to the Index view when an Id with the value -1 is passed to the Details() method.

________________________________________________________________________________________________
          using System.Web.Mvc;
          using Microsoft.VisualStudio.TestTools.UnitTesting;
          using Store.Controllers;
          namespace StoreTests.Controllers
          {
               [TestClass]
               public class ProductControllerTest
               {
                    [TestMethod]
                    public void TestDetailsRedirect()
                    {
                         var controller = new ProductController();
                         var result = (RedirectToRouteResult) controller.Details(-1);
                         Assert.AreEqual("Index", result.Values["action"]);

                    }
               }          
          }
________________________________________________________________________________________________

When you call the RedirectToAction() method in a controller action, the controller action returns a RedirectToRouteResult. The test checks whether the RedirectToRouteResult will redirect the user to a controller action named Index.


