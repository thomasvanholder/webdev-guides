Below are few common testing scenario's for rspec. 
To be able to use random sample data and to create test fixtures add to your `gemfile`

```ruby
group :development, :test do
  gem 'factory_bot_rails'
  gem 'faker'
end
```

- [User sign in](#user-sign-in)
- [User sign up](#user-sign-up)
- [User sign out](#user-sign-out)
- [forgot password](#forgot-password)

## User sign in
`spec/system/user_signin_spec.rb`

```ruby
require "rails_helper"

describe "User signs in", type: :system do
  scenario "valid with correct credentials" do
    user = create :user

    visit new_user_session_path

    fill_in "user_email", with: user.email
    fill_in "user_password", with: user.password
    click_button "sign in"

    expect(page).to have_text "Welcome back"
    find('#user-menu-button').click #click menu button to open dropw
    expect(page).to have_link "Sign out"
    expect(page).to have_current_path root_path
  end

  scenario "invalid with unregistered account" do
    build :user

    visit new_user_session_path

    fill_in "user_email", with: Faker::Internet.email
    fill_in "user_password", with: "FakePassword123"
    click_button "sign in"

    expect(page).to have_no_text "Welcome back"
    expect(page).to have_text "Invalid Email or password."
    expect(page).to have_no_link "Sign Out"
  end

  scenario "invalid with invalid password" do
    user = build :user

    visit new_user_session_path

    fill_in "user_email", with: user.email
    fill_in "user_password", with: "FakePassword123"
    click_button "sign in"

    expect(page).to have_no_text "Welcome back"
    expect(page).to have_text "Invalid Email or password."
    expect(page).to have_no_link "Sign Out"
  end
end
```

## User sign up  
`spec/system/user_signup_spec.rb`

```ruby
require "rails_helper"

describe "User signs up", type: :system do
  let(:email) { Faker::Internet.email }
  let(:password) { Faker::Internet.password(min_length: 8) }
  let(:subdomain) { "mysite" }

  before do
    visit new_user_registration_path
  end

  scenario "with valid data" do
    fill_in "user_email", with: email
    fill_in "user_password", with: password
    check "terms_agreement"
    click_button "Get Started"

    expect(page).to have_content("Welcome! You have signed up successfully.")
    expect(page).to have_text "Welcome back"
  end

  scenario "invalid when email already exists" do
    user = create :user

    fill_in "user_email", with: user.email
    fill_in "user_password", with: password
    check "terms_agreement"
    click_button "Get Started"

    expect(page).to have_no_text "Welcome back"
    expect(page).to have_text "Email has already been taken"
  end

  scenario "invalid without clicking agree terms" do
    fill_in "user_email", with: email
    fill_in "user_password", with: password
    click_button "Get Started"

    expect(page).to have_no_text "Welcome back"
    expect(page).to have_text "Terms of service must be accepted"
  end
end
```

## User sign out
 `spec/system/user_signout_spec.rb`

```ruby
require "rails_helper"

describe "User signs out", type: :system do
  before do
    user = create :user
    login_as(user)
    visit root_path
  end

  scenario "when user signed in" do
    click_link "Sign out"
    expect(page).to have_text 'You need to sign in or sign up before continuing.'
    expect(page).to have_no_link 'Sign Out'
  end
end
```

## Forgot password
`spec/system/forgot_password_spec.rb`

```ruby
require "rails_helper"

describe "User resets a password", type: :system do
  let(:non_existing_email) { Faker::Internet.email }

  before do
    visit new_user_password_path
    expect(page).to have_content "Reset your password"
  end

  scenario "user enters a valid email" do
    existing_user = create :user
    fill_in "user_email", with: existing_user.email
    click_button "Send me reset password instructions"

    expect(page).to have_text "You will receive an email with instructions"
    expect(page).to have_current_path new_user_session_path
  end

  scenario "user enters an invalid email" do
    fill_in "user_email", with: "username@example.com"
    click_button "Send me reset password instructions"

    expect(page).to have_text "Email not found"
  end

  scenario "user changes password" do
    token = create(:user).send_reset_password_instructions

    visit edit_user_password_path(reset_password_token: token)

    fill_in "New password", with: "Password123"
    fill_in "Confirm new password", with: "Password123"
    click_button "Change my password"

    expect(page).to have_text "Your password has been changed successfully."
    expect(page).to have_current_path root_path
  end

  scenario 'password reset token is invalid' do
    visit edit_user_password_path(reset_password_token: 'token')

    fill_in 'New password', with: 'p4ssw0rd'
    fill_in 'Confirm new password', with: 'p4ssw0rd'
    click_button 'Change my password'

    expect(page).to have_text 'Reset password token is invalid'
  end
end
```
