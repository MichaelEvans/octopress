---
layout: post
title: "Clickable Links in Android ListViews"
date: 2013-03-29 00:14
comments: true
categories: [Android, Tutorial, Code]
description: "Clickable Links in Android ListViews"
keywords: "Android, ListView, TextView, Linkify"
---

As part of my newest app ([Hacker News for Android](https://play.google.com/store/apps/details?id=com.evans.hackernews)), I needed to add the ability to click on links in a ListView, and have them open in the browser, while maintaining the ability for the user to click on the ListView row itself, and have the row listen to the click. 

If you try using `Linkify` and `LinkMovementMethod`, which you'd use on normal links in a TextView, you'll find that you can no longer use the ListView properly. 

<!-- more -->

The solution I'm using is slightly modified from this answer on StackOverflow [here](http://stackoverflow.com/a/7327332/458603).

{% codeblock [LinkifiedTextView.java] [lang:java] %}
import android.content.Context;
import android.text.Layout;
import android.text.Selection;
import android.text.Spannable;
import android.text.Spanned;
import android.text.style.ClickableSpan;
import android.util.AttributeSet;
import android.view.MotionEvent;
import android.widget.TextView;

public class LinkifiedTextView extends TextView {
	public LinkifiedTextView(Context context, AttributeSet attrs) {
		super(context, attrs);
	}

	@Override
	public boolean onTouchEvent(MotionEvent event) {
		TextView widget = (TextView) this;
		Object text = widget.getText();
		if (text instanceof Spanned) {
			Spannable buffer = (Spannable) text;

			int action = event.getAction();

			if (action == MotionEvent.ACTION_UP
					|| action == MotionEvent.ACTION_DOWN) {
				int x = (int) event.getX();
				int y = (int) event.getY();

				x -= widget.getTotalPaddingLeft();
				y -= widget.getTotalPaddingTop();

				x += widget.getScrollX();
				y += widget.getScrollY();

				Layout layout = widget.getLayout();
				int line = layout.getLineForVertical(y);
				int off = layout.getOffsetForHorizontal(line, x);

				ClickableSpan[] link = buffer.getSpans(off, off,
						ClickableSpan.class);

				if (link.length != 0) {
					if (action == MotionEvent.ACTION_UP) {
						link[0].onClick(widget);
					} else if (action == MotionEvent.ACTION_DOWN) {
						Selection.setSelection(buffer,
								buffer.getSpanStart(link[0]),
								buffer.getSpanEnd(link[0]));
					}
					return true;
				}
			}

		}

		return false;
	}
}
{% endcodeblock %}

Just drop this in as a replacement for your normal TextView, and you'll have the clicks on links in your text intercepted. Hope this helps a fellow developer!

---
Like this post? Any other Android tips you'd like to hear about? Let me know on [Twitter](https://twitter.com/m_evans10) or [Google Plus](https://plus.google.com/114052868601022948953/posts), or leave a comment.

