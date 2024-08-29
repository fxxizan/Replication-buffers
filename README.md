![image](https://github.com/user-attachments/assets/040303da-6303-49bd-8b7c-959b75798a28)

# Why is there so much reach and how do you mitigate it?
An explanation on why reach exists and how it can be mitgated

## Chapter I: Why is there so much reach?
Delay is an unavoidable aspect of online play. You can't play an online game without experiencing some form of delay at all. There will always be some time elapsed between your computer and the servers. However on roblox, this delay is much higher compared to other multiplayer games. Contrary to popular belief, ping isn't the only factor of why reach exists, and neither is it because roblox's servers are 'bad'. The biggest reason why reach is so prevalent on roblox is because of the massive replication buffer put in place. 
   
### What is a replication buffer?
Basically, a replication buffer is a delay added to physics updates, controlled by roblox itself. This exists in not just roblox, but most multiplayer games and also video platforms such as YouTube or TikTok. This buffer is in place to store future positions of the object so updates continue to be smooth incase any small lag happens. How I remember how this buffer smooths things out is with the youtube buffer (you can see this with the grey line on the playing line (shout out Dankus)).
![image](https://github.com/user-attachments/assets/ec20dd5e-590e-4e29-83d6-6921576bd757)
YouTube preloads future parts of the video that you haven't reached yet so you can watch the video seamlessly even if any small delays hit your wifi. This is why your video still plays on for around a minute after your wifi cuts out. If youtube didn't implement this buffer, then your video would constantly be stopping and playing again even if the smallest hit to your wifi occurs and some packets get delayed. Roblox implements a similar system to youtube (and so do other multiplayer games) with a buffer system where they guarantee that they have future positions of the character so they can use that if a small fluctuation in ping occurs momentarily. How this works in practice is:  
- Player A replicates their position to Player B
- Player B receives the signal and puts it in a table
- Instead of immediately setting Player A's position to the latest one received, Player B waits a while
- Player B updates Player A's position

This creates a small delay when Player A begins moving but afterwards this will be smooth as new signals will constantly be received and the delays will work with each other so the next update will be instant from the previous update (if that makes sense). Without a replication buffer, if Player A's signal isn't received at exactly the same ping as the previous signal, the player will freeze momentarily on Player B's client until the signal is received.

So after reading all this, you must think that massive reach is inevitable in order to keep the Player's positions and smooth, however this is not the case. Roblox's replication buffer is massive (there is a delay of around 0.2 seconds) when realistically, you only need a buffer of a few frames with modern games. There is also no way to edit (or even view) roblox's replication buffer length so reach cannot be fixed if you're planning to use stock roblox networking. Unfortunately, you will have to abandon roblox and create your own character replication.

## Chapter II: How do we fix reach?
Fixing reach isn't as hard as it may seem, however it is very annoying. Not only do you have to replicate the other player's positions manually and set up your own interpolation (smoothing) system, you have to do the same with the ball as that is also affected by the same massive replication buffer as your player (yes the ball is also in different positions between the network owner and other players). The ball being delayed as well as the player is quite interesting but this isn't an article talking about why the ball does a certain thing when something specific happens or why or anything like that lol. Simply, you have to also manually replicate the ball as well as the character's position if you want your game to look good. Here is a clip of the character being replicated without relying on roblox networking while the ball still relies on roblox networking:
https://youtu.be/07qMHy5DO00 (notice how the person at the start is dribbling while the ball is behind them and is constantly clipping through the ball). Here is a clip of the perspective of the other player (they're not walking through the ball lol) https://streamable.com/4u0brh

If you're fine with players somehow plagging the ball and dribbling with their back to the ball then sure you can ignore the ball as well, however you will still experience reach (you can still see some small reach in both clips (albeit not as much as normal)). This is because the ball is still not being replicated with the most up to date information so while you can see the other player's real position (for the most part), they do not see the real position of the ball so their hitbox will still pick that up and you'll still experience more reach than necessary as they can still tackle the ball from you when the ball is far away on your screen.

And.. there's still more we need to do to keep players at the latest states. Animations and humanoid state changes are also affected by this buffer (so it doesn't look like you're playing an animation early on other clients) so you also have to replicate that manually (luckily it's not hard at all compared to replicating the positions of the ball and players manually) (Fun fact: humanoid states are replicated after animations for some reasons (and quite considerably)! You can see this in most mps games where when you watch another player getup and they seem to go upside down before having their orientation corrected, this is because the animation offset makes them go upside down before the state update happens and their orientation is corrected)

After all that, we're done! (to my knowledge, I've done all this and I don't see anything else to do, but if there is anything extra then feel free to let me know)

## Chapter III: Conclusion
I hope you learnt about why reach is so massive on roblox and how it is actually possible to mitigate this. However, roblox should allow developers to control the length of replication buffers instead of expecting developers to rebuild their replication system. (If you're reading this and you can make feature requests on the dev forum then pls make a feature request to allow developers to control the replication buffer length)
