## Glassmorphism

https://glassmorphism.com

## Classnames (npm)

https://www.npmjs.com/package/classnames

A simple JavaScript utility for conditionally joining classNames together.

```react
var classNames = require('classnames');

class Button extends React.Component {
  // ...
  render () {
    var btnClass = classNames({
      btn: true,
      'btn-pressed': this.state.isPressed,
      'btn-over': !this.state.isPressed && this.state.isHovered
    });
    return <button className={btnClass}>{this.props.label}</button>;
  }
}
```

