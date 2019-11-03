# MVVM

1. Views know either which event they listen to or which property on the `VM` they bind to.
   - It seems like in the declarative XML, there is sudden mention of data, albeit it being view data.

2. **IMPORTANT** The UI is driven by an existing framework; e.g. `UIKit` for iOS. There is no way for the `VM` to send messages to the framework except using a "flag," which probably shouldn't be used as the source of truth.
   - Case: A tap on the button should show a popup.
     - Solution 1. The activity/view controller handles the event itself.
       - This is a bad solution since the activity is handling events, when it should only care about presentation.
     - Solution 2. The `V` raises the tap event, and the `VM` handles the event. The `VM` changes a flag such as `didDisplayPopup`, and the `V` observing that value shows the popup.
       - The flag is not an exact representation of the state. It's a duplicate state, contrived to solve the problem.
       - The flag also should not be considered as the source of truth; i.e. there can be a short moment where the flag is `true`, but the framework has not shown the popup.
       - The flag is a duplicate of the framework state. It should be reset when the `V` is affected, which is a hotbed for bugs.

3. Two components handle events.
  - UI events are handled by the `V` components; i.e. fragment, view, XML, etc.
  - Data events are handled by the `VM`.
  - Two separate sources to look for event handling. UI events could be a bit harder to find.