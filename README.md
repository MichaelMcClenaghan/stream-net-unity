stream-net for Unity
===========

This is a fork of shawnspeak's .NET getstream.io client that has been adapted for utilising UnityWebRequests.  As this project uses some C#6 features such as async/await, you need to be using the new .NET 4.6 equivalent runtime (experimental shipped in Unity 2017.1, stable in 2018.1).  This project also relies on the [Async Await Support](https://assetstore.unity.com/packages/tools/integration/async-await-support-101056) package which can be found on the Unity Asset Store, as well as the [Unity Supported fork of Newtonsoft.Json](https://github.com/SaladLab/Json.Net.Unity3D)

### Installation

Visit the [Releases](https://github.com/MichaelMcClenaghan/stream-net-unity/releases) section to download the latest .unitypackage and add it to your project.

### Usage

```c#
// Create a client, find your API keys here https://getstream.io/dashboard/
var client = new StreamClient("YOUR_API_KEY","API_KEY_SECRET");

// Reference a feed
var userFeed1 = client.Feed("user", "1");

// Get 20 activities starting from activity with id last_id (fast id offset pagination)
var results = await userFeed1.GetActivities(0, 20, FeedFilter.Where().IdLessThan(last_id));

// Get 10 activities starting from the 5th (slow offset pagination)
var results = await userFeed1.GetActivities(5, 10);

// Create a new activity
var activity = new Activity("1", "like", "3") 
{
	ForeignId = "post:42"
};  
userFeed1.AddActivity(activity);

// Create a complex activity
var activity = new Activity("1", "run", "1") 
{
	ForeignId = "run:1"
};
var course = new Dictionary<string,objects>();
course["name"] = "Shevlin Park";
course["distance"] = 10;
activity.SetData("course", course);
userFeed1.AddActivity(activity);

// Remove an activity by its id
userFeed1.RemoveActivity("e561de8f-00f1-11e4-b400-0cc47a024be0");

// Remove activities by their foreign_id
userFeed1.RemoveActivity("post:42", true);

// Let user 1 start following user 42's flat feed
userFeed1.FollowFeed("flat", "42");

// Let user 1 stop following user 42's flat feed
userFeed1.UnfollowFeed("flat", "42");

// Delete a feed (and its content)
userFeed1.Delete();

// Retrieve first 10 followers of a feed
userFeed1.Followers(0, 10);

// Retrieve 2 to 10 followers
userFeed1.Followers(2, 10);

// Retrieve 10 feeds followed by $user_feed_1
userFeed1.Following(0, 10);

// Retrieve 10 feeds followed by $user_feed_1 starting from the 10th (2nd page)
userFeed1.Following(10, 20);

// Check if $user_feed_1 follows specific feeds
userFeed1.Following(0, 2, new String[] { "user:42", "user:43" });


```
