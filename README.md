# node-red-contrib-dashboard-bar-chart-data

<h2>Bar-Chart-data</h2>
<p>Transforms <code>msg.payload</code> to be displayed in the <code>dashboard-chart</code> node as bar-chart. Can handle measurements (e.g. current power consumption) or meter readings (e.g. gas/water/energy meter count). 
  Calculates the sum, min or max in customizable time range</p>

<h3> Properties :</h3>
  <dl class="message-properties">
    <dt>Name
      <dd>The name of this node</dd>
    </dt>

<dt>x-interval
      <dd>1 bar per:
        <li>second</li>
        <li>minute</li>
        <li>quarter-hour</li>
        <li>hour</li>
        <li>day</li> 
        <li>year</li> 
      </dd>
    </dt>
  
<dt>x-size
    <dd>How many bars should be displayed. Example: x-interval=hours, x-size=24 : 24 bars, each 1 hour. That means that the last 24 hours will be displayed</dd>
  </dt>  
  
  <dt>Unit
    <dd>The unit of the payload, will be displayed in the bar-chart</dd>
  </dt>  

  <dt>Precision
    <dd>Number of decimals</dd>
  </dt>  
  
  <dt>Meter reading
    <dd>Set this to "True" if msg.payload is a meter reading, so that this node will use the difference of the current and last value.
      <li>True: msg.payload is meter reading, like the total energy, gas or water meter value or operating hours</li>
      <li>False: msg.payload is a measurement, like power (watt), pressure or temperature</li> 
    </dd>
  </dt>  
  
  <dt>Aggregate by
    <dd>
      <li>sum: sum all values in x-interval</li>
      <li>min: just display the smallest value in x-interval</li> 
      <li>max: just display the largest value in x-interval</li> 
    </dd>
  </dt>  
  </dl>

  <h3>Clear and Restore</h3>
  <dl>
    <dd>A message with the payload "clear" and the same topic like the sensor messages will reset the data storage (so that the chart will be blank again).<br>
      If you put the output of this node to the input of this node (via <code>persist node</code>), the data will be restored. 
      This could be helpful to avoid getting a blank bar-chart after node-red has been restarted (reboot).
    </dd>  
  </dl>
  
  
  <h3>Additional output: Sum, Min, Max, Settings</h3>
  <dl>
    <dd>This is included in the output message (and could be used to set chart title etc.):
      <li>The node settings (unit, x_interval, x_size, precision, is_meter_reading, agg_by) as json in <code>msg.settings</code></li>
      <li>The smallest value of all bars (min) in <code>msg.min</code></li>
      <li>The largest value of all bars (max) in <code>msg.max</code></li>
      <li>The sum of all bars in <code>msg.sum</code></li>
    </dd>  
  </dl>

  <h3>Sample Flow</h3>
  <dl>
    <dd>![sample flow](https://raw.githubusercontent.com/arneman/node-red-contrib-dashboard-bar-chart-data/master/img/sample_flow.png)
      
      <code>[{"id":"307f07f.8b8c1f8","type":"bar-chart-data","z":"3b18da32.bba756","name":"bar-chart-data","x_interval":"seconds","x_size":"24","unit":"Liter","precision":"0","is_meter_reading":"False","agg_by":"sum","x":340,"y":140,"wires":[["11edb254.d31a6e","3e6bc8b7.f4bc88"]]},{"id":"11edb254.d31a6e","type":"ui_chart","z":"3b18da32.bba756","name":"","group":"c716d8f1.4bb848","order":3,"width":"12","height":"4","label":"Liter letzte {{msg.settings.x_size}} Sekunden {{msg.data_sum}}","chartType":"bar","legend":"false","xformat":"HH:mm:ss","interpolate":"linear","nodata":"","dot":false,"ymin":"","ymax":"","removeOlder":"30","removeOlderPoints":"","removeOlderUnit":"60","cutout":0,"useOneColor":true,"colors":["#1f77b4","#aec7e8","#ff7f0e","#2ca02c","#98df8a","#d62728","#ff9896","#9467bd","#c5b0d5"],"useOldStyle":false,"outputs":1,"x":510,"y":100,"wires":[[]]},{"id":"3e6bc8b7.f4bc88","type":"persist in","z":"3b18da32.bba756","name":"store","storageNode":"e0e7ea5c.056af8","x":510,"y":180,"wires":[]},{"id":"8343e961.cbf7e8","type":"inject","z":"3b18da32.bba756","name":"sensor","topic":"devices/homie-har/water/liter","payload":"1","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":130,"y":100,"wires":[["307f07f.8b8c1f8"]]},{"id":"29a85329.d6b03c","type":"persist out","z":"3b18da32.bba756","name":"store","storageNode":"e0e7ea5c.056af8","x":130,"y":180,"wires":[["307f07f.8b8c1f8"]]},{"id":"c716d8f1.4bb848","type":"ui_group","z":"","name":"bar-chart","tab":"957fd14a.a1277","disp":true,"width":"12","collapse":false},{"id":"e0e7ea5c.056af8","type":"persist-store","z":"","filename":"~/store.json","interval":"60"},{"id":"957fd14a.a1277","type":"ui_tab","z":"","name":"bar-chart","icon":"dashboard","disabled":false,"hidden":false}]</code>    </dd>  
  </dl>
