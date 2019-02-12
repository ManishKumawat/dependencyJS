# DependencyJS
Dependency injection framework for Typescript and Javascript

## What is dependencyjs?
Dependencyjs is an dependency framework for javascript and typescript applications. This framework will help building independent components in application. So it is going to be useful if you want to achieve a well maintainable and testable application

## Installation

### Node.js

`dependencyjs` is available on [npm](http://npmjs.org). To install it, type:

    $ npm install dependencyjs

### Browsers

You can also use it within the browser; install via npm and use the `ComponentRegistry.js` file found within the download. For example:

```html
<script src="./node_modules/dependencyjs/src/ComponentRegistry.js"></script>
```

## Usage

You can register base classes with their implementation. Lets take an example of nodejs application

below is the base class

```ts
export abstract class BaseSample{
    public abstract ShowSample(name: string): string;
}
```

now create a concrete implementation of BaseSample class

```ts
export class Sample extends BaseSample{
    ShowSample(name: string): string{
        return "Test Name is :"+ name;
    }
}
```

Now register class with its concrete implementation


```ts
import {ComponentRegistry, registry} from "dependencyjs";
import {BaseSample} from "./BaseSample";
import {Sample} from "./Sample";

//register class object
let sample: BaseSample = new Sample();
registry.register<BaseSample>(BaseSample, sample);
```

Resolve class object to use it anywhere in code
```ts
let sample: BaseSample= registry.resolve<BaseSample>(BaseSample);
```

## Using resolver

Dependencies can be registered with extra parameter so that we can have multiple implementation of base class registered
in registry.

lets extend same Base sample in another class and register both of them in registry


```ts
export class SampleTypeSecond extends BaseSample{
    ShowSample(name: string): string{
        return "Test Name is :"+ name;
    }
}
```

Now register both of them in registry


```ts
import {ComponentRegistry, registry} from "dependencyjs";
import {BaseSample} from "./BaseSample";
import {Sample} from "./Sample";
import {Sample} from "./SampleTypeSecond";

//register class object
let sample: BaseSample = new Sample();
let secondSample: BaseSample = new SampleTypeSecond();

registry.register<BaseSample>(BaseSample, sample, "sample");
registry.register<BaseSample>(BaseSample, secondSample, "secondSample");
```


Resolve class object with resolver in your code
```ts
let sample: BaseSample= registry.resolve<BaseSample>(BaseSample, "sample");
let sampleSecond: BaseSample= registry.resolve<BaseSample>(BaseSample, "secondSample");
```


## Loading dependencies from configuration

dependencyjs provide a functionality where you can load classes dyanamically from xml configuration.

### How to configure xml

```xml
<container>
    <register type="<BaseClassType>" mapTo="<Concrete implementation class name>" resolver="<any string to identify class>">
        <typeProperty sourceType="<fs or package>" sourceInfo="<location of file or package name>"></typeProperty>
        <mapProperty sourceType="<fs or package>" sourceInfo="<location of file or package name>"></mapProperty>
    </register>
    <register type="<BaseClassType>" mapTo="<Concrete implementation class name>" resolver="<any string to identify class>">
         <typeProperty sourceType="<fs or package>" sourceInfo="<location of file or package name>"></typeProperty>
         <mapProperty sourceType="<fs or package>" sourceInfo="<location of file or package name>"></mapProperty>
    </register>
</container>
```

Details of each field

    1. Container: Container can have multiple class registration.
    2. Register: In register, you can mention detail about base class and child class to register the dependency.
        a. type: This will contain name of the exported base class. In our example about, it could be "BaseSample"
        b. mapTo: This will contain name of the exported concrete implementation class. In our example, it could be "Sample".
        c. typeProperty: This node will container information about base class.
            i. sourceType: You can use a base class from a node package or it could exist in your local system. So there are two possible
                           values -> "fs" or "package"
            ii. sourceInfo: If sourceType is package then this field will contain name of the package. If source type is "fs" then
                            this field will contain localtion of your file which contains base class. For example, location of Sample
                            class is "/BaseSample".
        d. mapProperty: This node will container information about concrete class.
            i. sourceType: You can use a concrete class from a node package or it could exist in your local system. So there are two possible
                           values -> "fs" or "package"
            ii. sourceInfo: If sourceType is package then this field will contain name of the package. If source type is "fs" then
                            this field will contain localtion of your file which contains concrete class. For example, location of Sample
                            class is "/Sample".

Here is example of one xml

```xml
<container>
    <register type="BaseSample" mapTo="Sample" resolver="Sample1">
        <typeProperty sourceType="fs" sourceInfo="/BaseSample"></typeProperty>
        <mapProperty sourceType="fs" sourceInfo="/Sample"></mapProperty>
    </register>
   <register type="BaseSample" mapTo="SampleTypeSecond" resolver="Sample2">
        <typeProperty sourceType="fs" sourceInfo="/BaseSample"></typeProperty>
        <mapProperty sourceType="fs" sourceInfo="/SampleTypeSecond"></mapProperty>
   </register>
</container>
```

### How to load xml configuration

In registry, a method has been introduced -

```ts
registry.loadConfiguration(config:Config).
 ```

You can use that method to load xml configuration. This method needs Config object as parameter. You can find it
from dependencyjs. Below are the detail of Config object

    Config: This class basically contains two properties.
        appDirectory: You need to initialize this property with main execution directory.
        configXmlFolder: This is the folder path where you can all xmls to register them in registry.

Below is the example of reading xml configuration.
```ts
import {registry, Config} from "dependencyjs";

let config: Config = new Config();
config.appDirectory = "Main application directory"
config.configXmlFolder = "where you contains all xmls"

registry.loadConfiguration(config);
```

All your classes will be register which has been mentioned in xmls. If not, you will get appropriate error message.

Now you can use registry as the way explained in usage section

### Core Contributors

Feel free to reach out to any of the core contributors with your questions or
concerns. We will do our best to respond in a timely manner.

[![IntimeTec](https://github.com/InTimeTecGitHub/)](https://github.com/InTimeTecGitHub/)
[![Manish Kumawat](https://github.com/ManishKumawat)](https://github.com/ManishKumawat)
