---
title: "Test Web Services Using SoapUI"
url: /howto9/testing/testing-web-services-using-soapui/
category: "Testing"
weight: 30
description: "Describes how to create a SOAP project with SoapUI, create assertions in SoapUI, and build an automated test using SoapUI."
tags: ["test", "testing", "web service", "soapui"]
---

## 1 Introduction

You can publish your own webservices in a Mendix application. These webservices consist of operations. Other applications can then call operations of this webservice and you can return a result. This result is based on a microflow that will be executed when the webservice is called. With SoapUI you can create (automated) tests for these webservices.

This how-to will teach you how to do the following:

* Create a SOAP project with SoapUI
* Create assertions in SoapUI
* Build an automated test using SoapUI

## 2 Preparation

Before you can start with this how-to, make sure you have completed the following prerequisites.

* Download and install [SoapUI](https://www.soapui.org/downloads/soapui.html)
* Complete the how-to [Exposing a web service](/howto9/integration/expose-a-web-service/)

| Software | Version used in this how-to |
| --- | --- |
| SoapUI | 5.2.0 |

{{% alert color="warning" %}}
All images, names and steps in this how-to are based on these versions. When using other versions the images and/or names on your screen may be different than the images or names used in this how-to.
{{% /alert %}}

## 3 Publishing More Microflows as Web Service Operation

In [How to Expose a Web Service](/howto9/integration/expose-a-web-service/) you already published the microflow *GetCustomers* as web service operation. In this chapter you will create two more microflow and publish them as web service operation.

1. Open Studio Pro.
2. Open the app you created in [How to Expose a Web Service](/howto9/integration/expose-a-web-service/).
3. Create a **Microflow** that creates a customer and returns a Boolean. 

    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/model1.jpg" >}}

4. Create a **Microflow** that deletes a customer and returns a Boolean.

    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/model2.jpg" >}}

5. Publish both microflows as web service operation of the **CustomerWebservice** as described in chapter 4 of [How to Expose a Web Service](/howto9/integration/expose-a-web-service/).

## 4 Creating a New SOAP Project

In this chapter you will create a new SOAP project.

1. Open **SoapUI**.
2. Press **CTRL+N** to create a new SOAP project.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580339.png" >}}
3. Run the app locally in Studio Pro.
4. Go to `http://localhost:8080/ws-doc/`.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580314.png" >}}
5. Enter the URL of the WSDL schema in the **Initial WSDL** field in SoapUI.
    The **Project Name** field will automatically be filled with the name of the webservice following by *?wsdl*.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580334.png" >}}
6. Click**OK**. This will create a new SOAP project in SoapUI.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580313.png" >}}

## 5 Building a TestSuite, TestCase, and TestStep

In this chapter you will build a testsuite. A testsuite contains one or more testcases. Every testcase contains one ore more teststeps. 

1. In SoapUI, press **CTRL+T** to create a new TestSuite.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580332.png" >}}
2. Click **OK**.
3. Select TestSuite 1 and press **CTRL+N** to create a new TestCase.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580331.png" >}}
4. Click **OK**.
5. Expand TestSuite 1 and TestCase 1.
6. Right-click **Test Steps(0)** and select **SOAP Request**.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580312.png" >}}
7. Enter *Retrieve Customers* in the name field.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580329.png" >}}
8. Click **OK**.
9. Select **CustomerWebserviceSoap -> GetCustomers**.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580328.png" >}}
10. Click **OK**.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580327.png" >}}
11. Click **OK**.
12. Open SOAP request **Retrieve Customers**.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580326.png" >}} 
13. Change the follow value:

    ```java
    <Offset>?</Offset>
    <PageSize>?</PageSize>
    ```

    into

    ```java
    <Offset>0</Offset>
    <PageSize>10</PageSize>
    ```

14. Press **ALT+Enter** to submit the request. The customers you created in the previous how-to will be shown in the response. The response will look like the code below:

    ```xml
    <soap:Envelope xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:tns="http://www.example.com/">
        <soap:Body>
            <tns:GetCustomersResponse>
                <Customer>
                    <Name>Jessica D. Rogers</Name>
                    <Address>2118 Collins Street</Address>
                    <ZipCode>PA 16646</ZipCode>
                    <City>Hastings</City>
                </Customer>
                <Customer>
                    <Name>Jamie S. Wentz</Name>
                    <Address>3269 Village View Drive</Address>
                    <ZipCode>MD 20872</ZipCode>
                    <City>Damascus</City>
                </Customer>
                <Customer>
                    <Name>Blake G. Race</Name>
                    <Address>4437 Sunny Glen Lane</Address>
                    <ZipCode>OH 44115</ZipCode>
                    <City>Cleveland</City>
                </Customer>
                <Customer>
                    <Name>Angela L. Dolly</Name>
                    <Address>3718 Alpha Avenue</Address>
                    <ZipCode>TX 75439</ZipCode>
                    <City>Ector</City>
                </Customer>
            </tns:GetCustomersResponse>
        </soap:Body>
    </soap:Envelope>
    ```

## 6 Assertions

Assertions are used to validate the message received by a TestStep during execution, usually by comparing parts of the message (or the entire message) to some expected value. In this chapter you will create an assertion that validates the number of customers.

1. Click **Assertions (1)**.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580307.png" >}}
    In chapter 3 step 9 the *Add SOAP Response Assertion* checkbox was checked. That is the first assertion you see in the list, *SOAP Response - VALID*.
2. Click the add assertion icon.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580324.png" >}}
3. Click **Property Content**.
4. Click **XPath Match**.
5. Enter *count(//Customer)* in the XPath Expression field.
6. In step 1.4 of the previous how-to you have added some data. Enter the number of created customers in the **Expected Result** field. In this example four customers were created.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580323.png" >}}
7. Click **Save**.

## 7 Extending Your Test Case

In this chapter you will extend your test case with multiple test steps and assertions.

1. Add a new SOAP request to the existing test case with the following details:

    | Field | Value |
    | --- | --- |
    | Specify name for step | Create Customer |
    | Select operation to invoke for request | CustomerWebServiceSoap -> CreateCustomer |

2. Change the request into the following value:

    ```xml
    <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:exam="http://www.example.com/">
       <soapenv:Header/>
       <soapenv:Body>
          <exam:CreateCustomer>
             <Name>Donald A. Hylton</Name>
             <Address>784 Better Street</Address>
             <City>Kansas City</City>
             <ZipCode>KS 66102</ZipCode>
          </exam:CreateCustomer>
       </soapenv:Body>
    </soapenv:Envelope>
    ```

3. Add a **Contains Assertion** and enter *true* in the content field.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580322.png" >}} 
4. Right-click test step **Retrieve Customers**.
5. Select **Clone TestStep**.

6. Change **TestStep Name** to *Retrieve Customers 2*.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580320.png" >}}
7. Click **OK**.
8. Open teststep **Retrieve Customers 2**.
9. Click **Assertions (2)**.
10. Open **XPath Match - Unknown**.
11. Change **Expected result** to *1 + [the number you entered in step 5.6]*.
12. Click **Save**.
13. Add a new SOAP request to the existing test case with the following details:
    * In the **Specify name for step**, enter *Delete Customer*.
    * In the **Select operation to invoke for request** field, enter *CustomerWebServiceSoap -> DeleteCustomer*.

14. Change the request into the following value:

    ```xml
    <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:exam="http://www.example.com/">
       <soapenv:Header/>
       <soapenv:Body>
          <exam:DeleteCustomer>
             <Name>Donald A. Hylton</Name>
          </exam:DeleteCustomer>
       </soapenv:Body>
    </soapenv:Envelope>
    ```

## 8 Running the Test Case

In this chapter you will learn how to run the testcase you created in previous chapters.

1. Open testcase **TestCase 1**.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580310.png" >}}
2. Click the run this testcase icon.
    {{< figure src="/attachments/howto9/testing/testing-web-services-using-soapui/18580309.png" >}}

Congratulations! You have just created your first automated test with SoapUI.

## 9 Read More

* [Finding the Root Cause of Runtime Errors](/howto9/monitoring-troubleshooting/finding-the-root-cause-of-runtime-errors/)
* [Clearing Warning Messages in Mendix](/howto9/monitoring-troubleshooting/clear-warning-messages/)
* [Testing web services using SoapUI](/howto9/testing/testing-web-services-using-soapui/)
* [Monitoring Mendix using JMX](/howto9/monitoring-troubleshooting/monitoring-mendix-using-jmx/)
* [Debugging Java actions remotely](/howto9/monitoring-troubleshooting/debug-java-actions-remotely/)
* [Log Levels](/howto9/monitoring-troubleshooting/log-levels/)
* [Debugging Java Actions](/howto9/monitoring-troubleshooting/debug-java-actions/)
* [Debugging Microflows and Nanoflows](/refguide9/debug-microflows-and-nanoflows/)
* [Debugging Microflows Remotely](/refguide9/debug-microflows-remotely/)

Learn more about this topic using the following helpful links:

* [10 tips for the SoapUI beginner](http://www.soapui.org/getting-started/10-tips-for-the-soapui-beginner.html)
