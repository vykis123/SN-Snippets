## Improve query Speed for read field operation by limiting returned field vlues

There is a work arround or cheat how you can improve your READ field value speed form tables if you only need some specific values form record.
This is by using GlideAggragate API together with groupBy method.

#### Example:

```javascript
const cmdbGr = new GlideAggragate("cmdb_ci");
cmdbGr.groupBy("name"); //Field value that you only want to fetch
cmdbGr.groupBy("sys_id"); //Include this to get all even repeated values
cmdbGr.query();

while (cmdbGr.next()) {
  gs.info(cmdbGr.getValue("name"));
}
```

#### Conducted tests on 200+k incident table with query speed results:

Simple glide record query takes about 2:49 min to complete query
![Example for simple glide query](./assets/glide%20R.PNG)

Glide Arragate with group by for number field takes 1:04 min to complete it
![Example for aggragate glide query](./assets//Glide%20A.PNG)

Glide Arragate with group by for number does not fetch other field values
![Example for aggragate glide query](./assets/Glide%20A%20only%201%20field.PNG)

You can also group the two fields to fetch 2 field values. So it is not only limited to 1 field. And also it does not impact the query speed.
![Example for aggragate glide query](./assets/Glide%20A%202%20fields.PNG)

But again it does not fetch more than needed fields
![Example for aggragate glide query](./assets/Glide%20A%202%20fields%20no%20more.PNG)

Test data example on incident table
![Example for tets data](./assets/incident%20list.PNG)

#### Few things to think aboout

<ul>
  <li>The more fields you will need, the more it will increase query speed so test it and verify if it realy needed;</li>
  <li>With GlideAggragate values can only be read but not updated/inserted;</li>
  <li>If the field that you want to get is not unique, but you need all values, then don't forget to groupBy unique field like, sys_id to get all fields;</li>
</ul>
