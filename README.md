# fluence_task1
<h2>Description</h2>
Follow the Fluence Quickstart Guide

<h2>Challenge Description</h2>
Extend the Fluence Quickstart, https://github.com/fluencelabs/examples/tree/main/quickstart/3-browser-to-service, with a distributed character count service deployed to at least one Fluence peer. Display a message's character count at the end of the message, e.g., (char count: 123 chars). You should accurately count characters.

<h2>Submission Requirements And Judging Criteria</h2>
Quality and comprehensiveness of documentation. Document your solution and submit it via a Github or GitLab repo with MIT or Apache 2.0 license.

<h2> Understanding </h2>
AS participants we need to go through the Fluence documentation thoroughly to understand its working and extent of applications. 
As a part of small test we have been assigned to create documentation such that, IFF anyone reads it in addition to the standard documentation, they should be able to easily grasp the idea of tweaking or ease of addition within the existing code to add a new service.

<h2> Solution and breakdown </h2>
Considering, we are at the stage as shown in documentation, creating following through quickstart guide.
<code>1-browser-to-browser</code>
<code>2-hosted-services</code>
<code>3-browser-to-service</code>
It is at this point we realize, we also need to add a total number of characters at the end of every message. 
<h4> Logic </h4>
Basically, count the number of characters in the message and add an additional statement like <code>char count: 00 chars</code>
<h6> How </h6> The easiest way to calculate the length of the message will be to use a default counting function like "message.len()", which by default gives the number of characters used in "message".
>Remember: We will counting an empty space as a character as well. It would be different if it had been specifically mentioned to not count empty space.
<h4> So, now that we have counted the number, we just need to add it to the end of every message </h4>
Which can also be translated as "Format the display of the message, so that it shows the number of characters in the message at the end of every message"
As we saw in <code>2-hosted-services</code>
>//code//
><code>
>#[marine]
pub fn hello(from: String) -> HelloWorld {
    HelloWorld {
        msg: format!("Hello from: \n{}", from),
        reply: format!("Hello back to you, \n{}", from),
    }
}
</code>
>//code//
the formating can be changed from here!
So, we just need to change it such that we keep adding count at the end.
>//code//
><code>
>#[marine]
pub fn hello(from: String) -> HelloWorld {
    HelloWorld {
        msg: format!("Hello from: {} char count: {} chars", from, (from.len()+12)),
        reply: format!("Hello back to you, {} char count: {} chars", from, (from.len()+19)),
    }
}
</code>
>//code//
As you see in the above piece of code, the statement is written such that there are two empty spaces, one for the actual message (from) and one for the count(from.len()). But we need to add a default value as we send a default template along side dynamic message in the combined form.
>You can choose to remove it, if you want the character count of only dynamic part.
You can also try a create a test for yourself before going ahead.
>//code//
><code>
>#[cfg(test)]
mod tests {
    use marine_rs_sdk_test::marine_test;

    #[marine_test(config_path = "../configs/Config.toml", modules_dir = "../artifacts")]
    fn char_count(hello_world: marine_test_env::hello_world::ModuleInterface) {
        let actual = hello_world.hello("testing done".to_string());
        assert_eq!(actual.msg, "Hello from: testing done char count: 24 chars");
        assert_eq!(actual.reply, "Hello back to you, testing done char count: 31 chars");
    }
}
</code>
>//code//
As you see, the string "testing done" is of "12" characters, we just added that to the default fourmula (from.len()+12) to estimate the result of hypothesis.
From here on, we need to follow the same steps from <code>./scripts/build.sh</code> to starting the service through <code> npm start </code> in <code> 3-browser-to-service </code> to display it in your browser.
>./scripts/build.sh
If you have created the test, use this command to compile it.
>cargo +nightly test --release
>mrepl configs/Config.toml
>marine aqua artifacts/hello_world.wasm
>fldist env
The above command will give a set of ids, you can choose anyone you want.
>fldist --node-id {selected_id} \
       new_service \
       --ms artifacts/hello_world.wasm:configs/hello_world_cfg.json \
       --name hello-world
The above command will generate a service id, which will be required to be updated in <code>3-browser-to-service/aqua/getting-started.aqua</code> file.

>npm run compile-aqua
>npm start
And you are done! You can now implment a character counting service such messaging! (or basically anywhere)
<p>you may see something of this sort.</p>
![Capture](https://user-images.githubusercontent.com/28018706/132922233-1a6df405-3dbd-435b-aa28-33c445582be2.PNG)
Enjoy and explore Fluence!
