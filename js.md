- Type validation.
  - To ensure that it only works
  with the desired type input.
  - Don't use `typeof`. Reference: [Fix the typeof operator](https://javascriptweblog.wordpress.com/2011/08/08/fixing-the-javascript-typeof-operator/).
  - Use `Object.prototype.call()`
    ```js
    Object.prototype.toString.call([1,2,3]); // "[object Array]"
    // or
    function betterTypeOf( input ){
      return Object.prototype.toString.call(input).match(/^[objects(.*)]$/)[1];
    }
    ```
- `parseInt` with base specified.
   ```js
   parseInt('20');     // returns 20
   parseInt('020');    // returns 16
   parseInt('0000020') // returns 16
   parseInt('020', 10) // returns 
   ```
   This function can be substituted with:
   ```
   Math.floor('020'); // returns 20
   Number('020');     // returns 20
   +'020'             // returns 20
- Compare explicit equality using `===`.
  Don't use `==`, as it considers the values to be equal eventhough they are different types.
- Cache the array length before using in the `for` / `while` loops.
  - Putting array length in the `for` loop condition cause the array length is unncesessarily re-accessed on each loop's iteration.
    ```js
    // Don't
    for (let i = 0; i < arr.length; i++) { /* do stuff */ }
    
    // Do
    const len = arr.length;
    for (let i = 0; i < len; i++) { /* do stuff */ }
    ```
- Use meaningful naming and don't worry if you have long variable names instead of saving a few keyboard stokes.
  - Don't add extra and unnecessary nouns. E.g.,
    ```js
    let nameString
    let theUsers
    ```
  - Make it easy to pronounce, so it takes less effort to process in mind.
    ```js
    // Don't
    let fName, lName, ctr
    
    // Do
    let firstName, lastName, counter
    ```
 - Function should do one thing and do it well (based on SOLID principle).
   ```js
   // Don't
   function getUserHandler(request, response) {
    const { userID } = request.params
    // inline SQL query
    select('user').where({id: userID}).first().then((user) => {
      response.json(user)
    })
   }
   
   // Do
   // User model (e.g., models/user.js)
   const tableName = 'user'
   const User = {
    getOne (userID) {
      return select('user').where({id: userID}).first()
    }
   }
   
   // handler (e.g., routes/user/get.js)
   function getUserHandler(request, response) {
    const { userID } = request.params
    User.getOne(userID).then((user) => {
      response.json(user)
    })
   }
   ```
 - Function name should be a vert or a verb phrase and it needs to communicate its intent as well as the order and intent of the arguments.
   ```js
   // Don't
   /**
    * Invite a new user with its email address
    * @param {String} user  User email address.
    */
    function inv(user) { /* do struff */ }
    
    // Do
    function inviteUser(emailAddress) { /* do struff */ }
    ```
 - Avoid long argument list and use single object paramenter assignment instead.
   ```js
   // Don't
   function registerUser(name, address, postalCode, phone, birthdate, gender, title) { /* do stuff */ }
   
   // Do
   function registerUser({ name, address, postalCode, phone, birthdate, gender, title }) { /* do stuff */ }
   registerUser({
    name: 'James',
    address: 'Clark Rd',
    postalCode: 876453,
    phone: 6999123,
    ....
   })
   ```
- Avoid implementing function that has side effect.
  ```js
  // Don't
  function addItemToCart(cart, item, quantity = 1) {
    const currentQuantity = cart.getItem(item.id) || 0
    cart.set(item.id, currentQuantity + quantity)
    return cart
  }
  
  // Do
  // This way the original cart won't be mutated.
  function addItemToCart(cart, item, quantity = 1) {
    const cartCopy = new Map(cart)
    const currentQuantity = cartCopy.getItem(item.id) || 0
    cartCopy.set(item.id, currentQuantity + quantity)
    return cartCopy
  }
  ``` 
- Higer level functions should be on top and lower level below. It makes it natural to read the source code.
  ```js
  // Don't
  function getFullName(user) { /* do stuff */ }
  function generateEmailTemplate(user) {
    const fullName = getFullName(user)
    return `Dear ${fullName}, ...`
  } 
  
  // Do
  function generateEmailTemplate(user) {
    const fullName = getFullName(user)
    return `Dear ${fullName}, ...`
  }
  function getFullName(user) { /* do stuff */ }
  ```
- Function should either query something or modify something, but not both.
- How to write nice Async code?
  ```js
  // Don't
  asyncFunc1((err, result1) => {
    asyncFunc2(result1, (err, result2)) => {
      asyncFunc3(result2, (err, result3) => {
        /* do stuff */
      })
    }
  })
  
  // Do
  asyncFuncPromise1()
    .then(asyncFuncPromise2)
    .then(asyncFuncPromise3)
    .then((result) => /* do stuff */)
    .catch((err) => /* do stuff */)
  ```
- Use promise and callback.
  ```js
  ```
  
   
   
    
 
 
 
