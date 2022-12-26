# Test REST API with Selenium WebDriver

Selenium is only for automating the web based applications.
However, if I want to do some data setup and teardown for the UI tests using Selenium, there are ways to do that by including some additional libraries. 
I can include REST API related testing in my existing Selenium framework.

## Challenges with UI Testing:
- Usually UI is slow. _This is because the browser first sends a request to a server for some information. As soon as it gets the required data, it might take some time processing the data and displaying it in a table (or any other appropriate format) by downloading images, applying the styles, etc. I have to wait for all these processes to complete before interacting with the app._
- Due to UI's slowness, it is time consuming.
- Same set of tests might be repeated on different browsers for cross-browser or cross-device testing.
- Browsers run as distinct processes, separate from the Selenium scripts. Hence, synchronization is always an issue.
- UI tests have a lot of dependencies, such as browsers, drivers, versions, options, grid, etc.

I try to do the API level testing as much as possible, in order to minimize coverage for UI testing.

## REST API Testing:
REST API testing is reasonably manageable, compared to Selenium WebDriver UI testing. Most of the APIs are one of GET / POST / PUT / PATCH / DELETE requests.

- GET - get data/info from the backend to show in the UI
- POST - add new info into the backend, crate a new record
- PUT - update/replace any existing info
- PATCH - partial update
- DELETE - delete/destroy the info from the backend

I normally use a testing framework like [Unittest](https://docs.python.org/3/library/unittest.html) (a.k.a. PyUnit) or [PyTest](https://docs.pytest.org/en/7.2.x/), when conducting UI testing with Selenium. 
I include APIs in the same framework for quick data setup, cleanup, assertions, etc.
I use a popular Python [Requests](https://requests.readthedocs.io/en/latest/) module.
_Requests_ is an elegant and simple HTTP library.

![image](https://user-images.githubusercontent.com/70295997/209491718-81463589-bb41-41ca-af3b-f07709ee434e.png)


