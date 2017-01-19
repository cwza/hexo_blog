title: Higher Order Component
categories: javascript
tags: [javascript, react]
date: 2017-01-19 14:48:57
---
A Simple Higher Order Component example

## Simple example
For loading spinner with different Component
``` js
// HOC declaration

function withLoadingSpinner(Component) {
  return function EnhancedComponent({ isLoading, ...props }) {
    if (!isLoading) {
      return <Component { ...props } />;
    }

    return <LoadingSpinner />;
  };
}

// Usage

const ListItemsWithLoadingIndicator = withLoadingSpinner(ListItems);

<ListItemsWithLoadingIndicator
  isLoading={props.isLoading}
  list={props.list}
/>
```

# Recompose
https://github.com/rafrex/spa-github-pages
A React Library for HOC
