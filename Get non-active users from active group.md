## Get non-active users from active groups

This is an example of query, how to find active groups, that have non-active users. This can be improved/changed/played arround to get diffrent results. But in general it is faster than using couple diffrent glideRecord queries.

P.S. I know getValue should not work with dot walk but in this case it does😛

```javascript
const memberGr = new GlideAggregate("sys_user_grmember");
memberGr.addQuery("user.active", false);
memberGr.addQuery("group.active", true);
memberGr.addAggregate("COUNT", "user");
memberGr.groupBy("group.name");
memberGr.query();

while (memberGr.next()) {
  const count = +memberGr.getAggregate("COUNT", "user");
  gs.info(`${memberGr.getValue("group.name")} (Non-active members: ${count})`);
}
```

#### Example result

![Example Result](./assets/Find%20active%20groups%20that%20contain%20non-active%20users.png)
