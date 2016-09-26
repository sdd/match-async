# match-async
react-router v4 Match component replacement that handles code split async components

```javascript
import React, { Component } from 'react';

const asyncComponent = path => class extends React.Component {

	componentWillMount() {
		if (!this.state) {
			require.ensure(path) // or System.import
				.then(module => {
					this.setState({ Component: module.default })
				});
		}
	}
	
	render() {
		const { Component } = this.state;
		return Component && <Component {props} />;
	}
}

class MatchAsync extends Component {

	constructor(props) {
		super(props);
		const { path } = this.props;
		this.state = { component: asyncComponent(path) }
	}
	
	render() {
		const { path, ...props } = this.props;
		const { component } = this.state;
		return <Match { ...{ ...props, component } } />;
	}
}

const App = () =>
  <BrowserRouter>
    <MatchAsync pattern="/foo" component="./Foo" />
    <MatchAsync pattern="/bar" component="./Bar" />
  </BrowserRouter>
```  
