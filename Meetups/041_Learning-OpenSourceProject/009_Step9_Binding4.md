## The passwordUpdated Method

- It is called when the user updates the content of the password field.
- When this method is called, the view model checks to see if the text entered represents a valid password.
- If it does, and the content of the username text field is also valid, then the view model asks the view controller to enable the Login button.
- Let's add the following tests:

```swift
fileprivate let validPassword = "D%io7AFn9Y"
fileprivate let invalidPassword = "a3$Am"


// MARK: passwordUpdated tests
extension LoginViewModelTests {
    
    func testPasswordUpdated_Calls_Validate_OnPasswordValidator() {
        let expectation = self.expectation(description: "expected validate() to be called")
        
        let viewModel = LoginViewModel(view: mockLoginViewController!)
        viewModel.passwordValidator = MockPasswordValidator(expectation, expectedValue: validPassword)
        
        viewModel.passwordUpdated(validPassword)
        
        self.waitForExpectations(timeout: 1.0, handler: nil)
    }
    
    func testPasswordUpdated_ValidPassword_UserNameValidated_EnablesLoginButton_OnViewController() {
        let expectation = self.expectation(description: "expected enableLogin(true) to be called")
        mockLoginViewController!.expectationForEnableLoginButton = (expectation, true)
        
        let viewModel = LoginViewModel(view: mockLoginViewController!)
        viewModel.userNameValidated = true
        viewModel.passwordUpdated(validPassword)
        
        self.waitForExpectations(timeout: 1.0, handler: nil)
    }
    
    func testPasswordUpdated_ValidPassword_UserNameNotValidated_DisablesLoginButton_OnViewController() {
        let expectation = self.expectation(description: "expected enableLogin(false) to be called")
        mockLoginViewController!.expectationForEnableLoginButton = (expectation, false)
        
        let viewModel = LoginViewModel(view: mockLoginViewController!)
        viewModel.userNameValidated = false
        
        viewModel.passwordUpdated(validPassword)
        
        self.waitForExpectations(timeout: 1.0, handler: nil)
    }
    
    
    func testPasswordUpdated_InvalidPassword_UserNameValidated_DisablesLoginButton_OnViewController() {
        let expectation = self.expectation(description: "expected enableLogin(false) to be called")
        mockLoginViewController!.expectationForEnableLoginButton = (expectation, false)
        
        let viewModel = LoginViewModel(view: mockLoginViewController!)
        viewModel.userNameValidated = true
        
        viewModel.passwordUpdated(invalidPassword)
        
        self.waitForExpectations(timeout: 1.0, handler: nil)
    }
    
    func testPasswordUpdated_InvalidPassword_UserNameNotValidated_DisablesLoginButton_OnViewController() {
        let expectation = self.expectation(description: "expected enableLogin(false) to be called")
        mockLoginViewController!.expectationForEnableLoginButton = (expectation, false)
        
        let viewModel = LoginViewModel(view: mockLoginViewController!)
        viewModel.userNameValidated = false
        
        viewModel.passwordUpdated(invalidPassword)
        
        self.waitForExpectations(timeout: 1.0, handler: nil)
    }
}
```

- Implement the needed changes in LoginViewModel

```swift
var userNameValidator:UserNameValidator?
var userNameValidated:Bool
var passwordValidated:Bool

init(view: LoginViewControllerProtocol) { 			
  self.userNameValidated = false 		
  self.passwordValidated = false
  super.init()
  self.view = view 
}

func userNameUpdated(_ value: String?) {
	guard let value = value else { 				
    view?.enableLoginButton(false) 
    return
	}

  let validator = self.userNameValidator ?? UserNameValidator() 
  userNameValidated = validator.validate(value)

  if userNameValidated == false { 
    view?.enableLoginButton(false) 
    return
	}

  if passwordValidated == false { 
    view?.enableLoginButton(false) 
    return
	}
  
	view?.enableLoginButton(true) 
}
```

- Run the tests.

