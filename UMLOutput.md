# Introduction #

In some circumstances it is useful to be able to visualize YANG models as UML diagrams.
Pyang supports two different ways of doing this:
  * Render png using [PlantUML](http://plantuml.sourceforge.net/): `pyang -f uml`
  * Generate XMI (soon) which can be imported to [ArgoUML](http://argouml.tigris.org/) : `pyang -f xmi`


## XMI for ArgoUML ##
I have given up on the I in XMI, the output only works for ArgoUML. Convince me that UML XMI is an interoperable standard.

```
pyang -f xmi ietf-netconf-acm.yang -o ietf-netconf-acm.xmi
```

Then you can start ArgoUML and chose XMI Import.
The easiest way is then to drag all or selected classes into the diagram and ask ArgoUML to auto-layout (Arrange - layout). After that, make the diagram look as you want.

The tagged values tab will show YANG specifics like `config, status, mandatory` etc. If you select a class
you get the tagged values for the container, list, if you select an attribute you get it for the leaf/leaf-list.


![https://pyang.googlecode.com/svn/trunk/doc/img/argo.png](https://pyang.googlecode.com/svn/trunk/doc/img/argo.png)



## PlantUML output ##

The uml plugin to pyang generates a file that can be read by [plantuml](http://plantuml.sourceforge.net/).

The following example converts the [ietf-netconf-monitoring](http://tools.ietf.org/html/rfc6022) module into a UML diagram:


```
$ pyang -f uml ietf-netconf-monitoring.yang -o ietf-netconf-monitoring.uml
$ java -jar plantuml.jar ietf-netconf-monitoring.uml 
$ open img/ietf-netconf-monitoring.png
```

On some platforms Java might spit out "java.lang.OutOfMemoryError: Java heap space"

Pass the `-Xmx<SIZE>m` to java like:

```
$ java -Xmx1024m -jar plantuml.jar ietf-netconf-monitoring.uml 
```


Please note that the uml output has numerous options to tweak and simplify the layout. To skip details try the `--uml-no=` parameter. In many cases you like to inline augments and groupings, use the `--uml-inline` parameters.

You can also pass several YANG files to the UML output format, but the files must be given in dependency order. (Multi-file does not work for module/sub-module dependancies)

![https://pyang.googlecode.com/svn/trunk/doc/img/monitor1.png](https://pyang.googlecode.com/svn/trunk/doc/img/monitor1.png)


Another example with some parts suppressed:
```
$ pyang -f uml --uml-no=circles,stereotypes,annotation ietf-netconf-monitoring.yang -o ietf-netconf-monitoring.uml
```

![https://pyang.googlecode.com/svn/trunk/doc/img/monitor2.png](https://pyang.googlecode.com/svn/trunk/doc/img/monitor2.png)

A multi-file example from the ongoing YANG modeling in [Metro Ethernet Forum](http://metroethernetforum.org/)

![https://pyang.googlecode.com/svn/trunk/doc/img/mef-8021ag_mef-soam-fm.png](https://pyang.googlecode.com/svn/trunk/doc/img/mef-8021ag_mef-soam-fm.png)

A complex example based on the [Configuration Data Model for IPFIX and PSAMP](http://www.rfc-editor.org/internet-drafts/draft-ietf-ipfix-configuration-model-08.txt)
![https://pyang.googlecode.com/svn/trunk/doc/img/ietf-ipfix-psamp.png](https://pyang.googlecode.com/svn/trunk/doc/img/ietf-ipfix-psamp.png)



There is also a python script to generate a UML package structure for all yangs in a folder.

`uml-utilities/uml-pkg`

Use it in the following way:
```
$ ls *.yang | xargs -n1 pyang -f depend > packages.txt
$ uml-pkg --title=TITLE --inputfile=packages.txt > packages.uml
$ java -jar plantuml.jar packages.uml
$ open img/packages.png


```