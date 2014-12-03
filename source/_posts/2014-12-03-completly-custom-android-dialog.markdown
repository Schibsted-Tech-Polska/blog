---
layout: post
title: "Completly custom android dialog"
date: 2014-12-03 10:25:00 +0100
comments: true
categories: [android, ui, android dialog]
published: true
author: Jacek Kwiecień
---

I’ve been digging through tons of solutions, that are obsolete or doesn’t work at all.
Let’s say we wan’t to completly custom dialog, that could look like this :

{% img [class names] http://www.aprilapps.com/wp-content/uploads/2014/10/Screenshot-168x300.png Custom android dialog %}

Please forgive the polish language, but I don't think it's relevant in this case.

First of you need some background for your dialog. 
I used a simple nine patch, pink background, light border. It named popup_bg

Second, you have to write a style for the dialog.
``` xml
<style name="Dialog" parent="@android:style/Theme.Dialog">
        <item name="android:windowBackground">@drawable/popup_bg</item>
        <item name="android:windowNoTitle">true</item>
        <item name="android:buttonStyle">@style/DarkButton</item> //if you want to style containing buttons
        <item name="android:textViewStyle">@style/TextView</item> // if you want to style containint text
    </style>
```

Then create a layout for your custom dialog. I named it dialog_leave_game.

Eventually you create the DialogFragment class

``` java
public class LeaveGameDialog extends DialogFragment {

	public interface ILeaveGameDialog {
		public void onLeaveGameConfirmed();
	}

	@Override
	public Dialog onCreateDialog(Bundle savedInstanceState) {
		Dialog dialog = new Dialog(getActivity(), R.style.Dialog);
		dialog.setContentView(R.layout.dialog_leave_game);
        ButterKnife.inject(this, dialog.getWindow().getDecorView()); //only if you use ButterKnife library

		return dialog;
	}

	@OnClick(R.id.leave_button)  //only if you use ButterKnife library
	protected void onLeaveClicked() {
		getListener().onLeaveGameConfirmed();
		this.dismiss();
	}

	@OnClick(R.id.stay_button)  //only if you use ButterKnife library
	protected void onStayClicked() {
		this.dismiss();
	}

}
```