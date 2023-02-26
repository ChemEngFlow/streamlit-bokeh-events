# The text below is a note of changes from the original. 
1. Fixing module errors (streamlit_bokeh_events/frontend/package.json)
[‘apache-arrow’ module not found error](https://discuss.streamlit.io/t/apache-arrow-module-not-found-error/21952) 
[Node.js 17.0.1 Gatsby error - "digital envelope routines::unsupported ... ERR_OSSL_EVP_UNSUPPORTED" [duplicate]](https://stackoverflow.com/questions/69665222/node-js-17-0-1-gatsby-error-digital-envelope-routinesunsupported-err-os)

2. Fixing Bokeh import error (streamlit_bokeh_events/frontend/public/index.html) 
[[bokehjs] Trying to integrate into a TypeScript project](https://discourse.bokeh.org/t/bokehjs-trying-to-integrate-into-a-typescript-project/5274)

# The text below is original.

# NOTE
Due to some personal issues I am unable to maintain the project for quite a while.
I am archiving this project and there wont be further updates in it.
Thanks to those who used the package. 

# Streamlit Bokeh Events
A streamlit component for bi-directional communication with bokeh plots.

#### Its just a workaround till streamlit team releases support for bi-directional communication with plots.

## Demo

#### Plots
![demo](assets/plots.gif)
---
#### Widgets

#### Data Tables
![demo2](assets/table.gif)

#### Color Picker
![demo3](assets/color.gif)

#### Run Any Custom JS
![demo4](assets/location.gif)

#### Get data from Clipboard
![demo5](assets/clipboard.gif)

## Installation

```bash
pip install streamlit-bokeh-events
```
## Usage

```python
import streamlit as st
from bokeh.plotting import figure
from bokeh.models import ColumnDataSource, CustomJS

# import function
from streamlit_bokeh_events import streamlit_bokeh_events

# create plot
p = figure(tools="lasso_select")
cds = ColumnDataSource(
    data={
        "x": [1, 2, 3, 4],
        "y": [4, 5, 6, 7],
    }
)
p.circle("x", "y", source=cds)

# define events
cds.selected.js_on_change(
    "indices",
    CustomJS(
        args=dict(source=cds),
        code="""
        document.dispatchEvent(
            new CustomEvent("YOUR_EVENT_NAME", {detail: {your_data: "goes-here"}})
        )
        """
    )
)

# result will be a dict of {event_name: event.detail}
# events by default is "", in case of more than one events pass it as a comma separated values
# event1,event2 
# debounce is in ms
# refresh_on_update should be set to False only if we dont want to update datasource at runtime
# override_height overrides the viewport height
result = streamlit_bokeh_events(
        bokeh_plot=p,
        events="YOUR_EVENT_NAME",
        key="foo",
        refresh_on_update=False,
        override_height=600,
        debounce_time=500)

# use the result
st.write(result)
```
