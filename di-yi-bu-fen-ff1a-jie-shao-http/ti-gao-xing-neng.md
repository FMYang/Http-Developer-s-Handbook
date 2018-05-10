#### Improving Performance

Performance is always a major concern for everyone involved in deploying Web content. The business needs driving development are often dependent on an efficient and robust implementation, and everyone involved in creating, deploying, and maintaining the application is responsible for meeting these needs.

Many improvements have been made to HTTP that enhance the performance and efficiency of the Web's infrastructure. Every unique application will benefit from a different set of ideas, but without knowing which types of methods are available for increasing performance and which situations are appropriate for using these methods, you will find it difficult to achieve your goals. Chapter 14 provides additional information concerning caching, connections, compression, and range transmissions.

In addition to the improvements made in HTTP itself, many common programming techniques can also serve to increase the performance of your application or at least allow you to support more users. For example, redirection is a technique that has become far too overused. After analyzing the pieces of an HTTP request in the next chapter, it should become clear that most information you need to provide the user with an appropriate response is included. If you redirect your users to a different URL just to display an error message, you have a failed design. Chapter 21 will go into more detail about intelligent architecture.

