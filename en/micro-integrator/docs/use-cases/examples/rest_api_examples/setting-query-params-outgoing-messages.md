# Setting query parameters on outgoing messages

REST clients use query parameters to provide inputs for the relevant
operation. These query parameters may be required to carry out the
back-end operations either in a REST service or a proxy service. Let’s
take a sample request that is sent to the
[SimpleStockQuoteService](https://docs.wso2.com/display/EI650/Setting+Up+the+ESB+Samples)
, and see how these parameters can be set in the outgoing message.

```
curl -v -X GET "http://localhost:8280/stockquote/view/IBM?param1=value1&param2=value2"
```

In this request, there are two query parameters (customer name and ID) that must be set in the outgoing message from the Micro Integrator. We can configure the API to set those parameters as follows:

### Synapse configuration

``` 
<api xmlns="http://ws.apache.org/ns/synapse" name="StockQuoteAPI" context="/stockquote">
       <resource methods="GET" uri-template="/view/{symbol}">
          <inSequence>
             <payloadFactory media-type="xml">
                <format>
                   <m0:getQuote xmlns:m0="http://services.samples">
                      <m0:request>
                         <m0:symbol>$1</m0:symbol>
                         <m0:customerName>$2</m0:customerName>
                         <m0:customerId>$3</m0:customerId>
                      </m0:request>
                   </m0:getQuote>
                </format>
                <args>
                   <arg evaluator="xml" expression="get-property('uri.var.symbol')"/>
                   <arg evaluator="xml" expression="get-property('query.param.param1')"/>
                   <arg evaluator="xml" expression="get-property('query.param.param2')"/>
                </args>
             </payloadFactory>
             <send>
                <endpoint>
                   <address uri="http://localhost:9000/services/SimpleStockQuoteService" format="soap11"/>
                   <property name="SOAPAction" value="getQuote" scope="transport"/>
                </endpoint>
             </send>
          </inSequence>
          <outSequence>
             <send/>
          </outSequence>
       </resource>
</api>                  
```
  
The query parameter values can be accessed through the "get-property"
function by specifying the parameter number as highlighted in the above
request.