pusher-redux
=================

Integration of Pusher into Redux
## Installation

You can download this by executing

<code>npm -i S pusher-redux</code>
## Usage

#### Configure Pusher
<pre>
<code>
import { configurePusher } from 'pusher-redux';
...
const options = { // options are... optional
  authEndpoint: '/authenticate/me'
}
const store = configureStore(initialState);
configurePusher(store, API_KEY, options);
</code>
</pre>

#### Use it in your component


<pre>
<code>
import { subscribe, unsubscribe } from 'pusher-redux';
import { NEW_ORDER } from '../pusher/constants';
...
export class MyPage extends React.Component {
  constructor(props, context) {
    super(props, context);
    this.subscribe = this.subscribe.bind(this);
    this.unsubscribe = this.unsubscribe.bind(this);
  }

  componentWillMount() {
    this.subscribe();
  }
  
  componentWillUnmount() {
    this.unsubscribe();
  }
  
  // upon receiving event 'some_event' for channel 'some_channe' pusher-redux is going to dispatch action NEW_ORDER
  // you can bind multiple actions for the same event and it's gonna dispatch all of them
  subscribe() {
    subscribe('some_channel', 'some_event', NEW_ORDER);
  }
  
  unsubscribe() {
    unsubscribe('some_channel', 'some_event', NEW_ORDER);
  }
  ...
}
</code>
</pre>

#### Change state in your reducer
<pre>
<code>
import { NEW_ORDER } from '../pusher/constants';
...
function orderReducer(state = initialState.orders, action) {
  case NEW_ORDER:
    return [...state, action.data.order];
  ...
}
</code>
</pre>


#### Format of actions
Pusher-redux dispatches actions of the following format:
<pre>
<code>
    return {
        type: actionType,
        channel: channelName,
        event: eventName,
        data: data
    };
</code>
</pre>

## Delayed configuration
Sometimes you want to authenticate user for receiving pusher information, but you don't have user credentials yet.
In this case you can do the following:
<pre>
<code>
import { delayConfiguration } from 'pusher-redux';
...
const options = { // options are... optional
  authEndpoint: '/authenticate/me'
}
const store = configureStore(initialState);
delayConfiguration(store, API_KEY, options);
</code>
</pre>

And once user information is available
<pre>
<code>
import { startConfiguration } from 'pusher-redux';
...
startConfiguration({ // pass options
  auth: {
    params: {
      auth_token: user.authToken
    }
  }
});
</code>
</pre>

### Options

Pusher-redux accepts all the same options that [pusher-js](https://github.com/pusher/pusher-js#configuration) does

## Contributing
You are welcome to import more features from [pusher-js](https://github.com/pusher/pusher-js)
## License

This code is released under the [MIT License](http://www.opensource.org/licenses/MIT).
