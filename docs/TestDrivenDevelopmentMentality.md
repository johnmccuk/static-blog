29/05/2017

## A brief example process on implementing TDD.

*<i class="fa fa-github" aria-hidden="true"></i> All files are available from the <a href="https://github.com/johnmccuk/test-driven-development-example" target="_blank">github repository</a>*.

I do write tests, lots of them. Its much easier to sleep at night knowing the code you've written does what you expect it to do. Theres no guarantee what you expect to happen is actually correct, but thats a different argument...

Writing tests is fundamentally different from [Test Driven Development (TDD)](http://www.agiledata.org/essays/tdd.html): 

* How many times have you had to refactor your code so thats its even testable?
* Test code that is more complex than the actual code that its testing? 
* Meaningless or superfluous tests that serve no purpose other than boosting code coverage? 
* Dropping tests due to time restraints.

If you to stick to best practices (Dependancy Injection[^1] , Single Responsibility Principle[^2]) life becomes much easier for testing, but you never get away from the mindset that you write the code, solve the problem, then test it as an afterthought.

Its very unintuitive to write the tests first, it stops you doing what you want to do, which is writing code. I'm much happier just working on the problem and refactoring as I get a deeper understanding of whats required. Surely it would be better to write testable code by testing it, rather than relying on my skill level to write testable code and then testing retrospectively? 

So I thought I would try and process how I plan to tackle TDD and stick to this mantra to see if I can rewire my brain into this approach.

* This concerns unit tests, these are the building block of integration.
* Tests should be simple and easy to understand
* Tests should initially throw errors as the code doesn't exist.
* Tests should then fail as the coding structure is put into place.
* Tests should then pass as each problem is solved.

To achieve this I need to:

* Think what I want to achieve.
* Write some tests to prove that I have achieved this.
* Write code that fulfils these assertions.
* Refactor
* Repeat

So lets set ourselves a very simple coding problem to run through this process:

* Some code to calculate the total of a shopping cart items
* We should get a total including tax
* We should get a total excluding tax

All files are available from the <a href="https://github.com/johnmccuk/test-driven-development-example" target="_blank">github repository</a>.

Normally I would think I require a class to deal with the shopping cart, so if there wasn't one, create it. So the only real difference at this point is that I'll create a test class:

* Create a class called ***ShoppingCart***.
* Create a test class called ***ShoppingCartTest***.

Now I should be thinking what I want to do, which is calculate the cart totals, including and excluding tax. So thinking this through, I have a ***ShoppingCart*** class, which should have a method to calculate totals. I'll name this method ***calculateCartTotals***.

This is obviously much simpler than most real world problems, but TDD is at the unit test level, so we should be testing at the individual method level anyway.

So thinking this through, I want to start with an empty cart:

```
    public function testShouldCalculateEmptyCartTotalWithTax()
    {
        $shoppingCart = new ShoppingCart([]);
        $result = $shoppingCart->calculateCartTotals();
        $this->assertEquals(0.00, $result['cartTotalIncludingTax']);
    }

    public function testShouldCalculateEmptyCartTotalWithoutTax()
    {
        $shoppingCart = new ShoppingCart([]); 
        $result = $shoppingCart->calculateCartTotals();
        $this->assertEquals(0.00, $result['cartTotalExcludingTax']);  
    }
```

So even at this point I have a clear coding goal. Tests fail mind you, but I can begin writing the required ***ShoppingCart*** methods:

```
class ShoppingCart
{
    protected $cartTotals = [
        'cartTotalExcludingTax' => 0.00,
        'cartTotalIncludingTax' => 0.00,     
    ];

    public function __construct(array $cartItems) {
        $this->cartItems = $cartItems;
    }

    public function calculateCartTotals($tax = 20)
    {
        foreach ($this->cartItems as $key => $cartItem) {
            $this->cartTotals['cartTotalExcludingTax'] += $cartItem;
            $this->cartTotals['cartTotalIncludingTax'] += $cartItem + $this->calculateTax($cartItem, $tax);
        }

        return $this->cartTotals;
    }

    protected function calculateTax($value, $tax)
    {
        return ((100 / $value) * $tax);
    }
}
```

Very simple code, but we already have external evidence that it works as expected.

```
Time: 26 ms, Memory: 2.75MB
OK (2 tests, 2 assertions)
...                                                              2 / 2 (100%)
```

We can now add more tests

```
    public function testShouldCalculateCartTotalWithTax()
    {
        $items = [
            'apples' => 1.20,
            'oranges' => 0.80
        ];
        $shoppingCart = new ShoppingCart($items);
        $result = $shoppingCart->calculateCartTotals();
        $this->assertEquals(2.40, number_format($result['cartTotalIncludingTax'], 2));
    }

    public function testShouldCalculateCartTotalWithoutTax()
    {
        $items = [
            'apples' => 1.20,
            'oranges' => 0.80
        ];  
        $shoppingCart = new ShoppingCart($items); 
        $result = $shoppingCart->calculateCartTotals();
        $this->assertEquals(2.00, number_format($result['cartTotalExcludingTax'], 2));  
    }
```

Now if we run our tests:

```
1) ShoppingCartTest::testShouldCalculateCartTotalWithTax
Failed asserting that '4,168.67' matches expected 2.2000000000000002.

FAILURES!
Tests: 4, Assertions: 4, Failures: 1.
```

Oh dear! Look on the bright side, the earlier bugs are caught in the development process, the better. Looking at the ***calculateTax*** method, I can see I should be dividing the value by 100 rather than the other way around.
```
    protected function calculateTax($value, $tax)
    {
        return ($value / 100) * $tax;
    }
```

```
4 / 4 (100%)
Time: 77 ms, Memory: 3.00MB
...
OK (4 tests, 4 assertions)
```

So now I know it works, I can carry on building on this, thinking what the next step would be, writing the test case, then the code.

***Think, Test, Code, Refactor, Repeat***

## Conclusion

So how does this differ from my normal approach and what have I gained?

* I would look over the specification and decide how to best achieve this.
* I would then decide how best to put this idea into practice within my code framework.
* Decide whats the output I expect is and create some test cases to confirm these expectations.
* Use a descriptive naming convention for creating classes, methods or functions as required. Use these descriptive names also for test cases.
* Write code, while frequently using `var_dump` to output results.
* Write code, running my test cases until they pass.
* When I was happy all seems to be working, write some tests to confirm I got the results I expected.
* Refactor until satisfied.
 
When you look at the procedure in this way, you could argue its actually quicker, we have no need to retrospectively write some tests and were only having to think a bit more laterally while deciding on a solution. Granted, you have to think 'backwards' from the test to the solution, but its not a huge step since you probably know what your output should be.

So what benefits do I get?

* Agile friendly repeatable process.
* It's harder to write code that doesn't stick to best practices such as dependancy injection.
* Problems and bugs appear earlier in the coding process. 
* I cant ignore testing due to laziness or time constraints.
* I have regression tests: when I add future changes, I can confidently check if I've broken anything.

Like any change, it takes mental effort to adhere to this way of working. But like all other changes, after a few hours or days it becomes the norm and you will have taken another step towards being a better software engineer.
