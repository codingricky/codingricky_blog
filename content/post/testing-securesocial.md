+++
author = "Ricky Yim"
categories = ["Play!", "Play", "Testing", "Authentication", "SecureSocial"]
date = 2013-08-05T02:00:00Z
description = ""
draft = false
slug = "testing-securesocial"
tags = ["Play!", "Play", "Testing", "Authentication", "SecureSocial"]
title = "Testing SecureSocial"
aliases = [
    "/testing-securesocial/"
]
+++

I started using the <a href="http://www.playframework.com/">Play2 framework</a> in conjunction with the <a href="http://securesocial.ws/">SecureSocial</a> authentication module. One of the features of the SecureSocial module is that it allows for Play controller actions to be annotated with @SecuredAction which ensures that the user is authenticated before the action is executed.<br />
<div>
<br /></div>
<div>
For e.g,</div>
<div>
<br /></div>
<div>
</div>
<pre class="prettyprint">public class MeetingsController extends Controller {

    private static final Form<meeting> meetingForm = Form.form(Meeting.class);

    @SecureSocial.SecuredAction
    public static Result newMeeting() {
        return ok(views.html.meetings.details.render(meetingForm, "[]"));
    }
}
</meeting></pre>
<pre><meeting>
</meeting></pre>
<pre><meeting>I started encountering problems when trying to test controllers using the framework.</meeting></pre>
<pre><meeting>
</meeting></pre>
<pre><meeting>For e.g,</meeting></pre>
<pre class="prettyprint">public class MeetingsControllerTest {
 
    @Test
    public void shouldReturnNewMeeting() {
        Result result = callAction(routes.ref.MeetingsController.newMeeting());
        assertThat(status(result)).isEqualTo(OK);
    }
}

</pre>
<br />
The first error I got was something along the lines of the application not being started.<br />
<br />
<br />
<pre class="prettyprint">Caused by: java.lang.RuntimeException: There is no started application
[error]     at scala.sys.package$.error(package.scala:27)
[error]     at play.api.Play$$anonfun$current$1.apply(Play.scala:51)
[error]     at play.api.Play$$anonfun$current$1.apply(Play.scala:51)
[error]     at scala.Option.getOrElse(Option.scala:120)
[error]     at play.api.Play$.current(Play.scala:51)
[error]     at securesocial.core.Authenticator$.cookieName$lzycompute(Authenticator.scala:188)
[error]     at securesocial.core.Authenticator$.cookieName(Authenticator.scala:188)
[error]     at securesocial.core.Authenticator$.<init>(Authenticator.scala:201)
[error]     at securesocial.core.Authenticator$.<clinit>(Authenticator.scala)</clinit></init></pre>
<pre><init><clinit>
</clinit></init></pre>
<pre><init><clinit>
</clinit></init></pre>
<pre></pre>
So wrapping the test in a fake application got rid of that error but....

<br />
<br />
<pre class="prettyprint">    @Test
    public void shouldReturnNewMeeting() throws Exception {
        running(fakeApplication(), new Runnable() {
            @Override
            public void run() {
                Result result = callAction(routes.ref.MeetingsController.newMeeting());
                assertThat(status(result)).isEqualTo(OK);
            }
        });
    }
</pre>
<br />
led to another problem<br />
<br />
<br />
<pre class="prettyprint">[error] Test MeetingsControllerTest.shouldReturnNewMeeting failed: expected:&lt;[200]&gt; but was:&lt;[303]&gt;
</pre>
This was caused by the application wanting to authenticate the user which is to be expected. So after a bit of googling the best viable solution was this post on Stackoverflow <a href="http://stackoverflow.com/questions/16244541/play-securesocial-developer-environment-and-unit-testing">http://stackoverflow.com/questions/16244541/play-securesocial-developer-environment-and-unit-testing</a>. Basically suggesting to remove the annotation before running the test using javassist.

So finally my end unit test looked like

<br />
<br />
<pre class="prettyprint">    @Test
    public void shouldReturnNewMeeting() throws Exception {
        removeAnnotationsFromMethodsInClass("controllers.MeetingsController");
        running(fakeApplication(), new Runnable() {
            @Override
            public void run() {
                Result result = callAction(routes.ref.MeetingsController.newMeeting());
                assertThat(status(result)).isEqualTo(OK);
            }
        });
    }

    private static void removeAnnotationsFromMethodsInClass(String clazz) {
        try {
            ClassPool pool = ClassPool.getDefault();
            CtClass cc = pool.get(clazz);
            CtMethod[] methods = cc.getDeclaredMethods();
            for (CtMethod method : methods) {
                AnnotationsAttribute attribute = (AnnotationsAttribute) method.getMethodInfo().getAttribute(AnnotationsAttribute.visibleTag);
                if (attribute != null) {
                    attribute.setAnnotations(new Annotation[]{});
                    method.getMethodInfo().addAttribute(attribute);
                }
            }
            cc.writeFile();
            cc.toClass();
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
</pre>

Admittedly, the remove annotations method is very crude as it just strips any method with an annotation off and can definitely be refined.

The test does run and pass. However I'm not entirely sure if there are any adverse ramifications of doing this. So this is what I'm sticking to until I find a better solution.