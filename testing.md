Below are few common testing scenario's for rspec.

- [User sign in](#user-sign-in)
- [User sign up](#user-sign-up)
- [User sign out](#user-sign-out)

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

