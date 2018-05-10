#### Utilizing Form Variables

Form variables are arguably the least popular method of state management and are only an option for those who rely on HTML form submissions to guide the navigation of their Web sites. This type of approach might exist in the following types of situations:

* The Web application is constructed in such a way that every page requires the users to input information into an HTML form, thus submitting a page each "step" of the way through the site.

* The interface is designed so that all navigation is provided by "buttons" that are really HTML form submissions, even though the users may not be providing data in an HTML form on every page.

In these types of scenarios, each request being sent from the Web client is a POST, so the option of including a form variable with the unique identifier is available. This can be accomplished with a hidden form variable that is included in each form:

`<input type="hidden" name="unique_id" value="12345">`
 
As I reiterate throughout this book, data from the client should never be trusted, and this case is no exception. I do not mean that all data sent from the client should be assumed to be false, because this assumption would make state management impossible. Rather, you should apply a skeptical approach to your analysis of this information. The use of multiple methods and/or multiple pieces of data is often found in state-manage-ment mechanisms to allow for cross-referencing and other techniques of double-check-ing the data being sent from the client. The sample given at the end of this chapter illustrates this point.