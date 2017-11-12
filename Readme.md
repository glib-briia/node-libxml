Node-LibXML
==========

LibXML2 Node.js Wrapper to Check: Wellformed, Validity against DTD, against XSD(schema), get doctype & getXpath elements

## Requirements: 

Node-Libxml has nos any dependencies, it's fully bundled, so you can just use it as it comes :)

Works on : Linux, Osx & Windows (no 32bits, just 64 bits) 

Node: Already built for all LTS node (4,6,8)

## Why use it 
Node-libxml has been thought differently than [libxmljs](https://github.com/libxmljs/libxmljs); whereas libxmljs can be use in commun uses, you can use node-libxml if:
- You wan to validate against DTD (libxmljs can't)
- You want a silent program if xml is not wellformed (node-libxml returns wellformed error in object; libxmljs throws)
- You want to do xml processing in parallel forks
- You want to validate against DTD / schema on multiple documents with just ONE dtd/schema loaded in memory (libxmljs loads it on each validation request), so it's clearly by far fastest!



## Install

```bash
  npm i node-libxml
```

## Use

```javascript
  const Libxml = require('node-libxml');
  let libxml = new Libxml();

  let xmlIsWellformed = libxml.loadXml('path/to/xml');
  console.log(xmlIsWellformed);
  console.log(xmlIsWellformed.wellformedErrors);

  console.log(libxml.getDtd());

  libxml.loadDtds(['path/to/dtd1', 'path/to/dtd2']);
  let xmlIsValid = libxml.validateAgainstDtd();
  console.log(xmlIsValid);
  console.log(xmlIsValid.validationDtdErrors);

  //Get some xpaths;
  let aRandomPathBoolean = libxml.xpathSelect('boolean(//some/path'));
  let aRandomPathNumber = libxml.xpathSelect('number(//some/path'));
  let countSomeElements = libxml.xpathSelect('count(//some/path'));
  //... & all xpath could do I think
```

check [tests](./test/libxml-test.js) for more examples 


## API


#### Loading functions



##### loadXml(string)
A function of libxml to load the XML file
`TAKE a path & RETURN true if wellformed | false if not`
`SET a an array 'wellformedErrors' in libxml element containing wellformed errors`


##### loadDtds(array)
A function of libxml to load the DTDs files
`TAKE an array of path of & RETURN nothing`
`SET a an array 'dtdsLoadedErrors' in libxml element IF error happend on loading dtd(s)`


##### loadSchemas(array)
A function of libxml to load the XSDs files
`TAKE an array of path of & RETURN nothing`
`SET a an array 'schemasLoadedErrors' in libxml element IF error happend on loading xsd(s)`
[ex](./test/libxml-test.js#L157)


#### Validating functions


##### validateAgainstDtd()
A function of libxml to validate against the previously loaded DTD(s)
`TAKE nothing & RETURN a string which is the name of the first dtd which has validated`
`RETURN false if no dtd(s) have validate the xml`
`RETURN null if no any dtd have been corectly loaded`
`SET a an array 'validationDtdErrors' in libxml element if no dtd has validate`

##### validateAgainstSchemas()
A function of libxml to validate against the previously loaded DTD(s)
`TAKE nothing & RETURN a string which is the name of the first dtd which has validated`
`RETURN false if no dtd(s) have validate the xml`
`RETURN null if no any schema have been corectly loaded`
`SET a an array 'validationDtdErrors' in libxml element if no dtd has validate`


#### Get informations of the the XML


#### xpathSelect(string)
A function of libxml to evaluate the xpath on the previously loaded XML
`TAKE string & RETURN value depending on what you asked (number, boolean..)`
`RETURN null if no match`
[ex](./test/libxml-test.js#L69)


#### Memory management
Use those functions when you have finished jobs of elements, or before overwrite them.
[ex](./test/libxml-test.js#100)


##### freeXml()
A function of libxml to free XML memory file
`TAKE nothing & RETURN nothin`
`FREE memory & clear 'wellformedErrors' property on libxml instance`


##### freeDtds()
A function of libxml to free all the DTD in memory
`TAKE nothing & RETURN nothin`
`FREE memory & clear 'dtdsLoadedErrors' property on libxml instance`
`FREE memory & clear 'validationDtdErrors' property on libxml instance`


##### freeSchemas()
A function of libxml to free all the Schema in memory
`TAKE nothing & RETURN nothin`
`FREE memory & clear 'schemasLoadedErrors' property on libxml instance`
`FREE memory & clear 'validationSchemaErrors' property on libxml instance`


##### clearAll()
A function of libxml to free all the memory taken by libxml2
`TAKE nothing & RETURN nothin`
free all memory in all instance in all fork using by libxml.

