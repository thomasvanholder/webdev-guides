Below are few common testing scenario's for rspec.

## User sign out

```ruby
require "rails_helper"

describe "User signs out", type: :system do
  before do
    user = create :user
    login_as(user)
    visit root_path
  end

  scenario "when user signed in" do
    find('#user-menu-button').click #click menu button to open dropw
    click_link "Sign out"
    expect(page).to have_text 'You need to sign in or sign up before continuing.'
    expect(page).to have_no_link 'Sign Out'
  end
end
```
