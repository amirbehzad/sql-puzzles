## Tips

### MySQL

```sql
-- extract username from an email address
SELECT SUBSTRING_INDEX('user.name@gmail.com', '@', 1);

-- extract email provider from an email address
SELECT SUBSTRING_INDEX('user.name@gmail.com', '@', -1);
```
