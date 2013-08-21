---
layout: post
title: "Parse Android To-do App Tutorial - Part Two - Users"
date: 2013-08-20 21:13
comments: true
categories: [Android, Tutorial, Parse, Development]
description: "Parse Android To-do App Tutorial - Part Two - Users"
keywords: "Android, Parse, Tutorial, Development"
---

Last week [I posted a tutorial](http://michaelevans.org/blog/2013/08/14/tutorial-building-an-android-to-do-list-app-using-parse/) on how to create a really basic Android To-do list application using [Parse](http://www.parse.com). It allowed a user to create tasks, and then toggle their completion status. Then, this information was synced to a server, so that the tasks will be kept up to date on multiple devices. The biggest issue with this app so far, was that there was no concept of users, so all tasks were shared among everyone. This week, we're going to introduce two new screens (which are very similar to one another), the registration and login screens.

<!-- more -->

### The Setup

The first thing we're have to do is two create two new activities, as I mentioned before. I called these `LoginActivity` and `RegisterActivity`. For simplicity sake, I'll assume that you used the New Activity Wizard, like we did last week to create the TodoActivity. You can also try the "LoginActivity" template from the wizard, but we'll skip that for this tutorial.

### Let's Code

Once you have both those activities set up, let's fill up their layouts. We need to add the following items to these layouts: two `EditText` fields for a username and password, two `TextViews` to be the labels for these inputs, and two `Buttons`, one to sign in/sign up, and one to switch from the sign up to sign in screen and vice versa. 

Here's what my registration layout looks like:

``` xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context=".RegisterActivity" >

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:gravity="center_vertical"
        android:orientation="vertical" >

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/username" />

        <EditText
            android:id="@+id/register_username"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:ems="10"
            android:inputType="text" >

            <requestFocus />
        </EditText>

        <TextView
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Password:" />

        <EditText
            android:id="@+id/register_password"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:ems="10"
            android:inputType="textPassword" />

        <Button
            android:id="@+id/sign_up"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="right"
            android:onClick="register"
            android:text="Sign Up" />
    </LinearLayout>

    <Button
        android:id="@+id/signInButton"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="17sp"
        android:onClick="showLogin"
        android:text="Already Have An Account? Log In!" />

</LinearLayout>
```

The login screen is very similar to that of the registration screen, with the same widgets in the same places, just different ids and labels, so we're just going to go over the registration screen in this tutorial and leave the login screen up as an exercise to the reader. (The code will be available on [Github](https://github.com/MichaelEvans/Parse-Android-Todo) if you get stuck). We're going to want to implement that `register` function in our RegisterActivity, so let's do that next.

{% img center /images/2013/08/20/login.png 300 500 Logging In %}

After declaring and binding variables to the two `EditText` fields (we'll call those `mUsernameField` and `mPasswordField`), we create our register function:

``` java
public void register(final View v){
	if(mUsernameField.getText().length() == 0 || mPasswordField.getText().length() == 0)
		return;

	v.setEnabled(false);
	ParseUser user = new ParseUser();
	user.setUsername(mUsernameField.getText().toString());
	user.setPassword(mPasswordField.getText().toString());

	user.signUpInBackground(new SignUpCallback() {
		@Override
		public void done(ParseException e) {
			if (e == null) {
				Intent intent = new Intent(RegisterActivity.this, TodoActivity.class);
				startActivity(intent);
				finish();
			} else {
				// Sign up didn't succeed. Look at the ParseException
				// to figure out what went wrong
				v.setEnabled(true);
			}
		}
	});
```

{% img center /images/2013/08/20/registering.png 300 500 Registering %}

What' were doing here is a) bailing out early if the user fails to fill in one of the fields, b) disabling the button while the network request is running, so the user can't try to register multiple times by accident, c) creating a ParseUser object with the username and password from the `EditText` fields, and then calling `signUpInBackground`, with a callback which is run when the request is complete. If there is an error for any reason, we re-enable the registration button, so that the user can fix any issues and resubmit. (We'll get to error handling a little later.) If there isn't an error, we start the TodoActivity that we [created last week](http://michaelevans.org/blog/2013/08/14/tutorial-building-an-android-to-do-list-app-using-parse/).

Huzzah! If you sign up, you now see a users table in your Parse console!

{% img center /images/2013/08/20/users.png 800 800 Users Console %}
### Making Things Belong

Now we need to make some changes to the TodoActivity. The first thing we need to do is make the query that fetches the data only return Tasks where the user is the same as the logged in user. We can do that by adding the following line that will modify our ParseQuery:
``` java
query.whereEqualTo("user", ParseUser.getCurrentUser());
```

We also need to make the Tasks belong to a user where they are created, which we can do by adding the following two statements when we are creating our new Tasks (we're also going to add the setters and getter for the user object on the Task model):

``` java
t.setACL(new ParseACL(ParseUser.getCurrentUser()));
t.setUser(ParseUser.getCurrentUser());
```

These lines will make the Task only read/writeable by the owner, and set the user associated with them to the current user. Take a peek in your Parse console, and you'll see the Tasks now have an owner!

{% img center /images/2013/08/20/tasks.png 800 800 Tasks now belong! %}

We also want to make it possible for a user to log out. We'll do this by providing an option in the action bar settings menu. We can do this by adding an entry in the `res/menu/todo.xml` file like the following:

``` xml
<item
    android:id="@+id/action_logout"
    android:orderInCategory="100"
    android:showAsAction="never"
    android:title="@string/action_logout"/>
```

Then all we need to do is wire up this action bar item. When options items are selected, there's a method that will be called in your activity named `onOptionsItemSelected`. We'll just fill this function out like this:

``` java
public boolean onOptionsItemSelected(MenuItem item) {
	switch (item.getItemId()) {
	case R.id.action_logout: 
		ParseUser.logOut();
		Intent intent = new Intent(this, LoginActivity.class);
		startActivity(intent);
		finish();
		return true; 
	} 
	return false; 
}
```
If the user selects the logout option, call the `logout()` method, and then send them back to the login screen.

The last thing we need to do in the TodoActivity is prevent a user from accessing this class while no one is logged in. We can do that back up in the `onCreate` method:

``` java
ParseUser currentUser = ParseUser.getCurrentUser();
if(currentUser == null){
	Intent intent = new Intent(this, LoginActivity.class);
	startActivity(intent);
	finish();
}
```

If there is no user currenty logged in, stop the TodoActivity, and forward us to the LoginActivity. 

### Error Handling

As I alluded to before, we're also going to want to add error handling. This way, if the user puts in bad credentials, or they try to register a username that's already taken, they'll get an informative error message. Let's do this by adding a `TextView` in our layout to display the message: (You'll also want to add a `colors.xml` file that will declare what `@color/red` is.)

``` xml
<TextView
	android:id="@+id/error_messages"
	android:layout_width="wrap_content"
	android:layout_height="wrap_content"
	android:layout_gravity="center_horizontal"
	android:textColor="@color/red"
	android:textAppearance="?android:attr/textAppearanceMedium" />
```

Then we'll bind a variable to that view, and add the following error handling to the else statement from our `signUpInBackground` call:

``` java
switch(e.getCode()){
case ParseException.USERNAME_TAKEN:
	mErrorField.setText("Sorry, this username has already been taken.");
	break;
case ParseException.USERNAME_MISSING:
	mErrorField.setText("Sorry, you must supply a username to register.");
	break;
case ParseException.PASSWORD_MISSING:
	mErrorField.setText("Sorry, you must supply a password to register.");
	break;
default:
	mErrorField.setText(e.getLocalizedMessage());
}
v.setEnabled(true);
```

This will set some nice messages that will explain what the error is, or fall back to the message that Parse sends us, if it's some other type of problem. 

{% img center /images/2013/08/20/error_handling.png 300 500 Pretty Error Handling %}

### Possibilities for Next Time

Next time we can take a look at adding some delete functionality, or maybe tabs for looking over completed/incomplete tasks. Another possiblity is adding social sign in, using Twitter and Facebook. Other suggestions are welcome!

**You can find the source on Github [here](https://github.com/MichaelEvans/Parse-Android-Todo).**

---
Like this post? Questions, concerns or mistakes? Any other Android tutorials you'd like to hear about? Let me know on [Twitter](https://twitter.com/m_evans10) or [Google Plus](https://plus.google.com/114052868601022948953/posts), or leave a comment below.

