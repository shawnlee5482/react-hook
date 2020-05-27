
1. Custom hook testing
We do not need to create wrapping component.
testing-library provides `renderHook`

https://kentcdodds.com/blog/how-to-test-custom-react-hooks

````````
import { renderHook, act } from "@testing-library/react-hooks";
import useCustomHook from './CustomHook';

describe("inital setting", () => {
    it("initial data is set as ", () => {
      const {result} = renderHook(() => useCustomHook('Hello'));
      expect(result.current.state).toStrictEqual('Hello');
    });

    it("set state works", () => {
        const {result} = renderHook(() => useCustomHook('Hello'));
        
        expect(result.current.state).toStrictEqual('Hello');
        act(() => {
            result.current.setCustomState('New State');
        });
        expect(result.current.state).toStrictEqual('New State');
    });

    it("clear state works", () => {
        const {result} = renderHook(() => useCustomHook('Hello'));
        expect(result.current.state).toStrictEqual('Hello');
        act(() => {
            result.current.clearCustomState();
        });
        expect(result.current.state).toStrictEqual('');
    });
});
`````````````

2. Testing a component which contains a custom hook

```
import React from 'react';
import { render } from '@testing-library/react';
import StateHook from './StateHook';
import { Provider } from 'react-redux';
import store from './redux/store';
import useCustomHook from './CustomHook'

jest.mock('./CustomHook');

test('Statehook shows the message', () => {
    useCustomHook.mockReturnValue({state: 'Mocked Return Value', setCustomState: () => {}});
    const { getByText } = render(
        <Provider store={store}>
            <StateHook/>
        </Provider>
    );
    const linkElement = getByText(/Mocked Return Value/i);
    expect(linkElement).toBeInTheDocument();
});
```
