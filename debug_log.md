# Debug Log

**Explain how you used the the techniques covered (Trace Forward, Trace Backward, Divide & Conquer) to uncover the bugs in each exercise. Be specific!**

In your explanations, you may want to answer:

- What is the expected vs. actual output?
- If there is a stack trace, what useful information does it contain?
- Which technique did you use, on which line numbers?
- What assumptions did you have about each line of code, and which ones were proven to be wrong?

_Example: I noticed that the program should show pizza orders once a new order is made, and that it wasn't showing any. So, I used the trace forward technique starting on line 13. I discovered the bug on line 27 was caused by a typo of 'pzza' instead of 'pizza'._

_Then I noticed another bug ..._

## Exercise 1

The program should be inserting a new order into the database, filled with the data entered in the form on the new order page. This doesn't happen, because of an error that happens when you submit an order. The error states that there is a TypeError and that 'topping' is an invalid keyword argument for PizzaTopping. This error happens on line 79 `pizza.toppings.append(PizzaTopping(topping=topping_str))`. Because I know the line number, I decided to trace backwards and found that the cause of the error is that the PizzaTopping class defines a `topping_type`property, *not* a `topping` property. 

Upon fixing this bug, I found another error. There was BuildError caused by line 84 `return redirect(url_for('/'))` because this is the wrong syntax for the `url_for` package. The correct syntax is to refernce the name of the python function, not the url route. To fix this I changed '/' to 'home'.

Now, the order claims to be submitted, but does not show up. This could be caused by errors in multiple locations so I will use Divide and Conquer to solve it. Starting at line 55 `return render_template('home.html', pizza_orders=all_pizzas)` I traced forward, noticing that the home.html template looked perfectly good. I then kept tracing forwards into the order.html template and /order function. This is where I noticed a discrepency between the names of the form input 'order_name' and the query parameter being accessed 'name.' The same issue existed with 'pizza_size' vs 'size,' I fixed these errors, and continued my trace. The next issue I came across was that there is a missing `db.session.commit` after line 81 `db.session.add(pizza)` After fixing this error, my order list is finally being populated.


## Exercise 2

The app is supposed to be displaying the weather in my city, but instead I am receiving an internal server error. The error occurs on line 52 `'city': result_json['name'],` and is a JSON KeyError, meaning that 'name' is not found as a key in our result_json object. To better understand this issue, I decided to trace back. I noticed that on line 45 the key was 'place' rather than 'q' as stated on the open weather maps API. There is nothing listed that claims that 'place' is a sufficient alias for 'q' so I decided to change it. Next, I checked to ensure we were receiving data from our form properly, and noticed that the form input names don't match up with the names we're trying to receive data from. I switched the code on lines 39 and 40 to match the names in the home.html template. This fixed the issue of name being a KeyError.

Next, I noticed that there was a new KeyError for the key 'temperature.' To fix this I referred back to the open weather maps API and noticed that the correct format for their JSON responses has the 'temp' key, not 'temperature.' This was an easy fix on line 54.

The next error which occurred was an 'invalid format string' error, occuring inside of the results.html template where we call `strftime()` 

## Exercise 3

[[Your answer goes here!]]
