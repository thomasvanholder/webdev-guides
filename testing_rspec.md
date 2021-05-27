Below are few common testing scenario's for rspec.

- [Sign in](#Sign-in)
- [Sign up](#Sign-up)
- [Sign out](#Sign-out)
- [Forgot password](#Forgot-password)
- [Invite user](#Invite-user)
 
To be able to use random sample data and to create test fixtures add to your `gemfile`
```ruby
group :development, :test do
  gem 'factory_bot_rails'
  gem 'faker'
end
```

## Sign in
`spec/system/signin_spec.rb`

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

## Sign up  
`spec/system/signup_spec.rb`

```ruby
require "rails_helper"

describe "User signs up", type: :system do
  let(:email) { Faker::Internet.email }
  let(:password) { Faker::Internet.password(min_length: 8) }

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

## Sign out
 `spec/system/signout_spec.rb`

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

## Invite User

`spec/system/invite_user_spec.rb`
- using [devise inviteable](https://github.com/scambra/devise_invitable)
- add `gem 'capybara-email'` to gem file to to read emails

```ruby
require "rails_helper"

describe "User invites a team member", type: :system do
  let(:email) { "test@example.com" }

  before do
    user = create :user
    login_as(user)
    visit team_members_path
  end

  scenario "sent email" do
    fill_in "user_email", with: email
    click_button "Invite Team Member"

    expect(page).to have_content "An invitation email has been sent to #{email}"
    expect(ActionMailer::Base.deliveries.count).to eq(1)
    expect(ActionMailer::Base.deliveries.last.to).to include(email)

    open_email(email)
    expect(current_email).to have_content 'Hello'
    current_email.click_link 'Accept invitation'
  end
end

describe "An invited user signs up", type: :system do
  let(:password) { Faker::Internet.password(min_length: 8) }

  before do
    user = create(:user)
    user.invite!
    visit accept_user_invitation_path(invitation_token: user.raw_invitation_token)
    expect(page).to have_content "Set your password"
  end

  scenario "with valid data" do
    fill_in "user_first_name", with: "Jim"
    fill_in "user_last_name", with: "Boom"
    fill_in "user_password", with: password
    fill_in "user_password_confirmation", with: password
    click_button "Set my password"

    expect(page).to have_text "Welcome"
  end

  scenario "with non-matching passwords" do
    fill_in "user_first_name", with: "Jim"
    fill_in "user_last_name", with: "Boom"
    fill_in "user_password", with: password
    fill_in "user_password_confirmation", with: 'anotherPW123'
    click_button "Set my password"

    expect(page).to have_content "Password doesn't match"
  end

  scenario "with blank passwords" do
    fill_in "user_first_name", with: "Jim"
    fill_in "user_last_name", with: "Boom"
    click_button "Set my password"

    expect(page).to have_content "Password can't be blank"
  end
end
```

Further reading:
- [Official Guide](https://github.com/thoughtbot/factory_bot/blob/master/GETTING_STARTED.md)
- [Better specs](https://www.betterspecs.org/#single)
- [Rspec style guide](https://github.com/rubocop/rspec-style-guide)
