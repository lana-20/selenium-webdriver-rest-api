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

Here is an example of how you might use the Python requests module to test a simple REST API:

    import requests

    # Send a GET request to the API
    response = requests.get("http://example.com/api/users")

    # Check the status code of the response
    assert response.status_code == 200

    # Parse the JSON response
    data = response.json()

    # Check the contents of the response
    assert len(data) > 0
    assert "id" in data[0]
    assert "name" in data[0]

    # Send a POST request to the API
    response = requests.post("http://example.com/api/users", json={
      "name": "Test User"
    })

    # Check the status code of the response
    assert response.status_code == 201

    # Parse the JSON response
    data = response.json()

    # Check the contents of the response
    assert "id" in data
    assert data["name"] == "Test User"

    # Send a PUT request to the API
    response = requests.put("http://example.com/api/users/{}".format(data["id"]), json={
      "name": "Updated Test User"
    })

    # Check the status code of the response
    assert response.status_code == 200

    # Send a DELETE request to the API
    response = requests.delete("http://example.com/api/users/{}".format(data["id"]))

    # Check the status code of the response
    assert response.status_code == 204

This example sends a GET request to the /api/users endpoint to retrieve a list of users, a POST request to the same endpoint to create a new user, a PUT request to update the newly created user, and a DELETE request to delete the user. The requests module makes it easy to send HTTP requests and parse the response, whether it is in JSON format or some other format.

Here is an example of how you might use the Python requests module and Selenium to test a simple REST API as part of a larger automated test:

    from selenium import webdriver
    import requests

    # Start the webdriver and navigate to the login page
    driver = webdriver.Chrome()
    driver.get("http://example.com/login")

    # Log in to the application
    driver.find_element(By.ID, "username").send_keys("testuser")
    driver.find_element(By.ID, "password").send_keys("testpass")
    driver.find_element(By.XPATH, "//button[text()='Log in']").click()

    # Send a GET request to the API
    response = requests.get("http://example.com/api/users", headers={
      "Authorization": "Bearer {}".format(driver.get_cookie("access_token")["value"])
    })

    # Check the status code of the response
    assert response.status_code == 200

    # Parse the JSON response
    data = response.json()

    # Check the contents of the response
    assert len(data) > 0
    assert "id" in data[0]
    assert "name" in data[0]

    # Send a POST request to the API
    response = requests.post("http://example.com/api/users", headers={
      "Authorization": "Bearer {}".format(driver.get_cookie("access_token")["value"])
    }, json={
      "name": "Test User"
    })

    # Check the status code of the response
    assert response.status_code == 201

    # Parse the JSON response
    data = response.json()

    # Check the contents of the response
    assert "id" in data
    assert data["name"] == "Test User"

    # Send a PUT request to the API
    response = requests.put("http://example.com/api/users/{}".format(data["id"]), headers={
      "Authorization": "Bearer {}".format(driver.get_cookie("access_token")["value"])
    }, json={
      "name": "Updated Test User"
    })

    # Check the status code of the response
    assert response.status_code == 200

    # Send a DELETE request to the API
    response = requests.delete("http://example.com/api/users/{}".format(data["id"]), headers={
      "Authorization": "Bearer {}".format(driver.get_cookie("access_token")["value"])
    })

    # Check the status code of the response
    assert response.status_code == 204

    # Close the webdriver
    driver.quit()

This example uses Selenium to log in to the application and retrieve an access token from a cookie, which is then passed in the headers of the API requests as an authorization token. The requests module is used to send the GET, POST, PUT, and DELETE requests to the API, and to parse the response. This allows you to include API testing as part of a larger automated test of the application.

----

[5 Best Python Libraries for working with HTTP](https://www.yeahhub.com/5-best-python-libraries-working-http/)

[The Best Python HTTP Clients For 2022](https://www.scrapingbee.com/blog/best-python-http-clients/)

[Selenium WebDriver – How To Test REST API](https://www.vinsguru.com/selenium-webdriver-how-to-test-rest-api/)

[Requests - HTTP library for Python](https://requests.readthedocs.io/en/latest/)

[REST-ASSURED WITH CUCUMBER: USING BDD FOR WEB SERVICES AUTOMATION](https://angiejones.tech/rest-assured-with-cucumber-using-bdd-for-web-services-automation/)
