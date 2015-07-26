# Cloudformation Stack Outputs

| Staging | Production |
|:-:|:-:|
|[![Build Status](http://drone.stocktio.com/api/badge/github.com/Stockflare/lambda-stack-ouputs/status.svg?branch=master)](http://drone.stocktio.com/github.com/Stockflare/lambda-stack-ouputs)| --- |

Parameter discovery between Cloudformation is difficult and whilst there are lots of tools out there (even our own [Launcher gem](http://github.com/Stockflare/launcher), at Stockflare we prefer to use this lambda function to facilitate a more environment and sub-environment agnostic way of determining which parameters from which cloudformations are correct.

After launching this Cloudformation, you can integrate a token into your Cloudformation with a resource similar to the one below:

**Note:** The parameter `StackOutputsArn` has been declared elsewhere in the template. Or in our case, we use the [launcher gem](http://github.com/Stockflare/launcher) to automatically discover it.

```
"Network": {
  "Type": "Custom::StackOutputs",
  "Properties": {
    "ServiceToken": { "Ref" : "StackOutputsArn" },
    "StackName" : "network"
  }
}
```

As we can see from the example above, the `StackName` parameter is used to determine which Cloudformation the outputs are coming from. In this case, we use the stack named "network" to create attributes for the "Network" resource. Once this function completes we can then retrieve these attributes like so:

```
{ "Fn::GetAtt": [ "Network", "PrivateSubnetA" ] }
```
