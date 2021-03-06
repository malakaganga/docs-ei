# Using Namespaces

Data services are implemented using XML, which means that there can be
conflicting element names. Therefore, we use namespaces to uniquely
identify the elements defined within a specific web service. Using
namespaces for a data service is optional. However, in certain data
services, namespaces become necessary. Read below for more information.

!!! info

Namespace inheritance in a data service:

Namespaces can be defined at different levels of a data service, which
means that you can have multiple namespaces applicable to your data
service at the same time. In such a scenario, the top-level namespaces
will be inherited by the lower level as explained below.

1.  The [namespace at the data service
    level](#UsingNamespaces-Settingthenamespaceforadataservice) applies
    to all aspects of the data service, i.e., to all elements in the
    results of all the queries (output mapping).
2.  The [namespace at the query result level (row
    namespace)](#UsingNamespaces-Settingthenamespaceforadataservicequery)
    overrides the service namespace and applies a given namespace to all
    the result elements (output mapping elements) of that query.
3.  The [namespace specified for a result element (output
    mapping)](#UsingNamespaces-Settingnamespacesforspecificelementsinthequeryresult)
    overrides the query-level row namespace.
4.  If you have a [complex element with child elements
    nested](#UsingNamespaces-Settingnamespacesforcomplexresults) :
    1.  The namespace for the complex element overrides the result row
        namespace and assigns the namespace to all its child elements.
    2.  The namespace specifically assigned to a child element overrides
        the parent element's namespace.


See the following topics for instructions on setting namespaces at
different levels of a data service:  

-   [Setting the namespace for a data
    service](#UsingNamespaces-Settingthenamespaceforadataservice)
-   [Setting the namespace for a data service
    query](#UsingNamespaces-Settingthenamespaceforadataservicequery)
-   [Setting namespaces for specific elements in the query
    result](#UsingNamespaces-Settingnamespacesforspecificelementsinthequeryresult)
-   [Setting namespaces for complex
    results](#UsingNamespaces-Settingnamespacesforcomplexresults)

### Setting the namespace for a data service

The service namespace is used to uniquely identify a web service, which
means that this namespace applies to all elements in the data service.
In the WSDL that represents the service, the namespace is indicated by
the "targetNamespace" attribute. The service namespace can be set when
you define a data service using the management console as shown below.

![](http://docs.wso2.org/wiki/download/attachments/4886292/figure01.png?version=1&modificationDate=1329153263000){width="450"}

### Setting the namespace for a data service query

When you define a query for your data service, you can set a namespace
for the elements in the query result (output mapping). That is, the
**Row Namespace** applies to all the result rows that you define as
output mappings for that query. The example below shows how the row
namespace is set for the query result element: *Customers* .

![](http://docs.wso2.org/wiki/download/attachments/4886292/figure02.png?version=1&modificationDate=1329154036000){width="650"}

After setting the row namespace, the data type of the **Customers**
element falls under the given namespace as shown below.

![](http://docs.wso2.org/wiki/download/attachments/4886292/figure03.png?version=1&modificationDate=1329155065000)

!!! info

If you have multiple queries with the same element wrapper but different
element rows, be sure to use separate namespaces for each query. The
example below illustrates two queries in a data service (.dbs file) with
the following qualities:

-   The element wrapper is the same for both queries, which is
    `           <result element="employees" rowName="employee">          `
    .
-   The two queries contain two unique element rows: '
    `           salary          ` ' and ' `           phone          `
    '.

Therefore, since we are using the same element wrapper for both queries,
we need to use two separate namespaces t o identify the two conflicting
elements ('salary' and 'phone') in the two queries as shown below.

``` java
    <query id="employeesByNumberSQL" useConfig="default">
          <sql>select * from Employees where employeeNumber = ?</sql>
          <result defaultNamespace="http://abc.com/empNamespace1" element="employees" rowName="employee">
             <element column="lastName" name="last-name" xsdType="string"/>
             <element column="firstName" name="first-name" xsdType="string"/>
             <element column="email" name="email" xsdType="string"/>
             <element column="salary" name="salary" xsdType="double"/>
          </result>
          <param name="employeeNumber" ordinal="1" sqlType="INTEGER"/>
    </query>
    <query id="employeesByIDSQL" useConfig="default">
          <sql>select * from Employees where employeesID = ?</sql>
          <result defaultNamespace="http://abc.com/empNamespace2" element="employees" rowName="employee">
             <element column="lastName" name="last-name" xsdType="string"/>
             <element column="firstName" name="first-name" xsdType="string"/>
             <element column="email" name="email" xsdType="string"/>
             <element column="phone" name="phone" xsdType="string"/>
          </result>
          <param name="employeeID" ordinal="1" sqlType="INTEGER"/>
    </query>
```
    

### Setting namespaces for specific elements in the query result

The row namespace applies to all elements in the query result.
Alternatively, you can specify namespaces for selected elements (output
mapping elements) in the query result as shown below. This namespace
will only apply to that element.

![](http://docs.wso2.org/wiki/download/attachments/4886292/figure04.png?version=1&modificationDate=1329155298000){width="500"}

This is how the setting is reflected in the WSDL.

![](http://docs.wso2.org/wiki/download/attachments/4886292/figure05.png?version=1&modificationDate=1329155352000)

!!! info

If you have a data service query using the same result element multiple
times, be sure to use separate namespaces for each element to avoid
conflicts. The example below illustrates a single query in a data
service (.dbs file) that has the same element defined twice. In this
situation, we have to use separate namespaces for the two elements in
order to distinguish them separately.  

``` java
    <query id="getProductByIDSQL" useConfig="default">
          <sql>select * from Products where productID = ?</sql>
          <result defaultNamespace="http://abc.com/productIDNamespace" element="products" rowName="product">
             <element column="productID" name="productID" xsdType="string"/>
             <element column="productID" name="productID" xsdType="INTEGER"/>
             <element column="productName" name="productName" xsdType="string"/>
          </result>
    </query>
```
    

### Setting namespaces for complex results

In complex results, the result elements can be arbitrarily nested. These
nested elements can have their own namespaces. The example given below
illustrates a query with a complex result (nested elements). As shown
below, the namespace specified for the 'contact' element is inherited by
the child elements that are nested within. However, if you assign
specific namespaces to the child elements, the namespace inherited by
the parent element will be overridden.

``` java
    <query id="customersInBostonSQL" useConfig="default">
          <sql>select  * from Customers where city = 'Boston' and country = 'USA'</sql>
          <result defaultNamespace="http://abc.com/nsb" element="customers" rowName="customer">
             <element column="customerName" name="customer-name" xsdType="string"/>
             <element namespace="http://abc.com/nsb" name="contact">
                <element column="contactLastName" name="contact-last-name" xsdType="string"/>
                <element column="contactFirstName" name="contact-first-name" xsdType="string"/>
             </element>
             <element column="phone" name="phone" xsdType="string"/>
             <element column="city" name="city" xsdType="string"/>
             <element column="country" name="country" xsdType="string"/>
          </result>
    </query>
```

The WSDL created for the above service is as follows:

![](http://docs.wso2.org/wiki/download/attachments/4886292/figure07.png?version=1&modificationDate=1329156299000)

The resulted XML of the executed service is as follows:

![](http://docs.wso2.org/wiki/download/attachments/4886292/figure08.png?version=1&modificationDate=1329156360000)
