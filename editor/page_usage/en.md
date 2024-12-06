# Page usage

Below are several practices on how you can use pages. You can combine these practices.

### Free switching between pages

If you want to have two or more pages in your graphical interface and switch between them in arbitrary order.

To achieve this, create a common page where you place all the page switches, and create pages for the elements that will be displayed and hidden.

The common page should always remain visible. Make this page the starting page. Also, make one of the pages for the elements the starting page; this page will be displayed when launching the graphical interface.

Change the properties of the page switches so that when clicked, both the common page and the page with the corresponding elements are displayed, and all other pages are hidden.

This way, the common page will always remain visible, and the switches for all pages will be visible on any page.

![en_07](en_07.jpg)

### Additional parameters page with return button

You can move additional information to a separate page that opens when necessary.

To do this, place page switches on the main page leading to the additional page. On the additional page, place a page switch leading back to the main page. Make its appearance similar to a Back button.

![en_08](en_08.jpg)

### Controlling visibility of elements

Using pages, you can control the visibility of an element or a group of elements from the controller.

To do this, place the elements on a separate page whose visibility you want to control. Assign a variable to this page. By changing the variable in the controller, you can show or hide the elements directly from the controller.

![en_06](en_06.jpg)













