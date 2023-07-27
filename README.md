# Week 11-Lab Manual
## Deployment to Azure

### A. Azure portal
1. Login to portal.azure.com with your college credentials.
2. Click Create a Resource >  Web App
    - You can create a New Resource Group and give it a name.
    - Provide your webapp a name
    - Choose ***.NET 6*** in Runtime. 
    - Got to the next screen, Review and Create.
3. Then click on ***Go to Resource*** and click on ***Default Domain*** to see if your page is live.

4. Then Click on *Download publish profile* and save it at a safe location.

### B. Visual Studio

5. Open up the project you would like to deploy in your Visual Studio

6. Right click on your Project and click ***Publish***


7. In the window that opens, click ***Import Profile*** and click Next.

8. In the next window, click Browse and choose the file you downloaded in Step 4.

9. Click Finish and then Publish.

## Stripe Payment Integration

1. Create an account with create an account Stripe

2. In your dashboard, you can find your publishable key and Secret Key. Copy it as we will be using it in the next steps. 

3. Click on Products tab and create a few products of your choice. Note down, the product ID of each of these products. 

4. Open your visual studio and go the project where you want to install the payment option.

5. Install stripe NuGet package.

6. Create 3 View(cshtml) pages for checkout, success and cancel. Then, Visit the link : https://stripe.com/docs/checkout/quickstart and copy the code for the corresponding files and paste it into your files.

7. In HomeController.cs, create methods to Invoke these files such as

   ```
    public IActionResult Checkout()
        {
            return View();
        }

        public IActionResult Success()
        {
            return View();
        }

        public IActionResult cancel()
        {
            return View();
        }
   ``` 

9.  Go to HomeController.cs and paste the following **inside HomeController class**. Remember to replace your secret key and product ID. Remove the angle brackets as well.

```
[Route("create-checkout-session")]
        [HttpPost]
        public ActionResult Create()
        {
            var domain = "https://localhost:7073"; //replace with your local port number
            var options = new SessionCreateOptions
            {
                LineItems = new List<SessionLineItemOptions>
                {
                  new SessionLineItemOptions
                  {
                    // Provide the exact Price ID (for example, pr_1234) of the product you want to sell
                    Price = "<your price id>",
                    Quantity = 1,
                  },
                },
                Mode = "payment",
                SuccessUrl = domain + "/Home/Success",
                CancelUrl = domain + "/Home/cancel",
                
            };
            StripeConfiguration.ApiKey = "<your secret key>";
            var service = new SessionService();
            Session session = service.Create(options);

            Response.Headers.Add("Location", session.Url);
            return new StatusCodeResult(303);
        }
```

9. Run the application and test out the payment functionality using some test payment cards provided at https://stripe.com/docs/testing
