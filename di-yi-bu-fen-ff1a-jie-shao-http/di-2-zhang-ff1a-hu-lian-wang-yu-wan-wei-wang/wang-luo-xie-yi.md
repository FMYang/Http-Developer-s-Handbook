#### Networking Protocols

When somepeople speak about networking protocols, a reference is made to the Open Systems Interconnection \(OSI\) model. With this model, all of the networking protocols are categorized into one of seven types of protocols, where each type is represented visually as a layer. The visualization of layers is intended to help people understand the relationship between the various protocols and illustrate their dependencies. As it turns out, the only feature of the OSI model that has gained much respect or popularity is the idea of protocols represented as layers. However, the use of these specific seven layers is rarely seen as more than a theoretical academic model, and most people use a four-layermodel.

#### Protocol Layers

The Internet hasgiven us open standards and a worldwide practical implementation of those standards. Most people visualize networking as a four-layer model, with IP being a specific layer \(rather than just an option\), because most modern networks are connected to the Internet in some fashion. The four layers as they relate to HTTP are illustrated in[Figure 2.2](itss://chm/0672324547_ch02lev1sec3.html#ch02fig002).

##### Figure 2.2. The four-layer protocolmodel.

#####  How Protocols Work Together

IP providesthe foundation for networking on the Internet. Because of this, it appears as one of the bottom layers in networking, with the physical layer being the only layer it depends on. Everything else is built on top of IP. Specifically, the Transport Layer and Application Layer protocols are messages contained in IP packets.

For example, consider[Figure 2.3](itss://chm/0672324547_ch02lev1sec3.html#ch02fig003). If we consider HTTP a container for Web content such as HTML, the dependency of HTML on HTTP becomesclear.

##### Figure 2.3. Visualization of an HTTP response.

In this regard, HTTP is sometimes referred to as a wrapper for Web content. Usingthis perspective,[Figure 2.4](itss://chm/0672324547_ch02lev1sec3.html#ch02fig004)illustrates protocol dependencies in an alternativefashion.

##### Figure 2.4. An alternativefour-layer protocol model.

As I discuss in the following chapter, HTTP's dependency on TCP, Transmission Control Protocol, is not very well described by the visualization of layers. TCP's involvement is more of a wrapper for HTTP in terms of the series of events involved in an HTTP transaction rather than each specific message, because TCP is responsible for maintainingthe state of connections.

#### The Role of HTTP

HTTP'sresponsibility is with Web content, which is why it is an important topic for Web developers. It is a mistake to consider it just another networking protocol, because it has very little to do with networking in fact and everything to do with the Web itself.

HTTP provides the environment for Web content, much like IP provides the environment for HTTP. Its responsibility is to provide whatever ancillary information is necessary to adequately request Web content as well as deliver it. As it turns out, this ancillary information is extremely important with Web applications, as it is often used to dynamically generate the actualcontent.



