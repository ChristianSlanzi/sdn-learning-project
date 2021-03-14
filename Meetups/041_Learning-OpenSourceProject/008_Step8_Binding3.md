## The userNameUpdated Method

- It is called when the user updates the content of the username field.
- When this method is called, the view model checks to see iif the text entered represents a valid username.
- If it does, and the content of the password text field is also valid, then the view model asks the view controller to enable the Login button.
- Let's add the following tests:

```swift
fileprivate var validUserName = "abcdefghij" fileprivate var invalidUserName = "a"


// MARK: userNameUpdated tests
extension LoginViewModelTests {
  func testUserNameUpdated_Calls_Validate_OnUserNameValidator() {
    let expectation = self.expectation(description: "expected validate() to be called")

    let viewModel = LoginViewModel(view: mockLoginViewController!)
    viewModel.userNameValidator = MockUserNameValidator(expectation, 
                                                        expectedValue: validUserName)

    viewModel.userNameUpdated(validUserName)

    self.waitForExpectations(timeout: 1.0, handler: nil)
  }

  func testUserNameUpdated_ValidUserName_PasswordValidated_EnablesLoginButton_OnViewController() {
    let expectation = self.expectation(description: "expected enableLogin(true) to be called")
    mockLoginViewController!.expectationForEnableLoginButton = (expectation, true)

    let viewModel = LoginViewModel(view: mockLoginViewController!)
    viewModel.passwordValidated = true
    viewModel.userNameUpdated(validUserName)

    self.waitForExpectations(timeout: 1.0, handler: nil)
  }

  func testUserNameUpdated_ValidUserName_PasswordNotValidated_DisablesLoginButton_OnViewController() {
    let expectation = self.expectation(description: "expected enableLogin(false) to be called")
    mockLoginViewController!.expectationForEnableLoginButton = (expectation, false)

    let viewModel = LoginViewModel(view: mockLoginViewController!)
    viewModel.passwordValidated = false

    viewModel.userNameUpdated(validUserName)

    self.waitForExpectations(timeout: 1.0, handler: nil)
  }

  func testUserNameUpdated_InvalidUserName_PasswordValidated_DisablesLoginButton_OnViewController() {
    let expectation = self.expectation(description: "expected enableLogin(false) to be called")
    mockLoginViewController!.expectationForEnableLoginButton = (expectation, false)

    let viewModel = LoginViewModel(view: mockLoginViewController!)
    viewModel.passwordValidated = true

    viewModel.userNameUpdated(invalidUserName)

    self.waitForExpectations(timeout: 1.0, handler: nil)
  }

  func testUserNameUpdated_InvalidUserName_PasswordNotValidated_DisablesLoginButton_OnViewController() {
    let expectation = self.expectation(description: "expected enableLogin(false) to be called")
    mockLoginViewController!.expectationForEnableLoginButton = (expectation, false)

    let viewModel = LoginViewModel(view: mockLoginViewController!)
    viewModel.passwordValidated = false

    viewModel.userNameUpdated(invalidUserName)

    self.waitForExpectations(timeout: 1.0, handler: nil)
  }
}
```



- Xxx

- xxx

  ```swift
  code
  ```

- fix the production code

  ```swift
  code
  ```

- add more tests

  ```swift
  code
  ```

- The tests are green, so the TDD of the SignupViewModel is completed.

