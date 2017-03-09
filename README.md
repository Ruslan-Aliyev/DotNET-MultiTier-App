# .NET MultiTier App

Simple demonstration of how to make N-Tier application: WCF service reading from XML file. 
MVC presents the read data. XML file contain list of students and students' ID numbers. 
This N-Tier application searches student by ID number.

![Directories](https://github.com/Ruslan-Aliyev/DotNET-MultiTier-App/raw/master/Illustrations/directories.PNG)

![XML file](https://github.com/Ruslan-Aliyev/DotNET-MultiTier-App/raw/master/Illustrations/xml.PNG)

![New WCF project](https://github.com/Ruslan-Aliyev/DotNET-MultiTier-App/raw/master/Illustrations/createService.PNG)

### The Interface and WCF service file should look like this:

### IService1.cs

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.Serialization;
using System.ServiceModel;
using System.Text;

namespace WcfServiceLibrary1
{
	[ServiceContract]
	public interface IService1
	{

		[OperationContract]
		string GetStudentName(string number);  

	}
}
```

### Service1.cs

```cs
using System.Collections.Generic;
using System.Linq;
using System.Runtime.Serialization;
using System.ServiceModel;
using System.Text;
using System.Web;
using System.Xml.Linq;

namespace WcfServiceLibrary1
{
	public class Service1 : IService1
	{

		public string GetStudentName(string number)
		{
			string studentname = "";
			XDocument doc = XDocument.Load("C:\\Users\\User\\Desktop\\StudentDB.xml");
			var data = from str in doc.Descendants("student")
					   where str.Attribute("number").Value == number
					   select str.Attribute("name").Value;

			foreach (string str1 in data)
			{
				studentname = str1;
			}
			return studentname;
		}
	}
}
```

### Right click the solution line in the solution explorer and create new MVC project:

![](https://github.com/Ruslan-Aliyev/DotNET-MultiTier-App/raw/master/Illustrations/createApp.PNG)

### In the MVC project, right click References and add the WCF service:

![](https://github.com/Ruslan-Aliyev/DotNET-MultiTier-App/raw/master/Illustrations/Linking.PNG)

### Put the following code into the HomeController.cs:

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.Mvc;

namespace MvcApplication1.Controllers
{
	[HandleError]
	public class HomeController : Controller
	{
		ServiceReference1.Service1Client obj = new ServiceReference1.Service1Client();
		public ActionResult Index()
		{
			ViewData["Message"] = obj.GetStudentName("02");

			return View();
		}

		public ActionResult About()
		{
			return View();
		}
	}
}
```				

### For example, the following indicates what should be done if we are looking for the student with ID number of 02

![](https://github.com/Ruslan-Aliyev/DotNET-MultiTier-App/raw/master/Illustrations/example.PNG)

### Right click the MVC Application project and set it as the start-up project

### Run the project. The result should be like below:

![](https://github.com/Ruslan-Aliyev/DotNET-MultiTier-App/raw/master/Illustrations/result.PNG)
