# N8n Tutorial: Step-by-Step Guide

## Flow Order Webhook → Fetch All Posts → Filter by User ID → Return only the Titles:
1. Webhook1 – receives userId
- Method: POST
- Path: /filter-posts
- Respond: Using "Respond to Webhook" Node ✅
- (Go to Settings tab → Change “Respond” to that)

2. HTTP Request – fetches posts
- Name: Get All Posts
- Method: GET
- URL: https://jsonplaceholder.typicode.com/posts
- Response Format: JSON
- Leave everything else default

3. Code – filters the posts using the userId from Webhook1
```
// Get userId from Webhook input (first node)
const userId = $items("Webhook1")[0].json.body.userId;

// Get all posts from HTTP Request node
const posts = items.map(item => item.json);

// Filter posts by userId and extract titles
const titles = posts
  .filter(post => post.userId === userId)
  .map(post => post.title);

// Return the final formatted object
return {
  json: {
    success: true,
    userId,
    titles
  }
};
```

4. Respond to Webhook Node
```
```

## Code
### If You Receive a Single Object
```
{
  "name": "John",
  "email": "john@example.com"
}
```

- Use: Run Once for Each Item (✅ Default Mode). You process just one item. Use item.json
```
return {
  json: { ... }
};
```

### If You Receive an Array (Multiple Items)
```
[
  { "name": "John", "email": "john@example.com" },
  { "name": "Jane", "email": "jane@example.com" },
  { "name": "Jack", "email": "jack@example.com" }
]
```

- Use: Run Once for All Items. You need access to the whole items list
```
items.map(item => {
  const data = item.json;
  return {
    json: {
      nameUpper: data.name.toUpperCase()
    }
  };
});
```

Data Type     | Mode You Use	       | Access        | Return Type
------------- | -------------          | ------------- | -------------
Single object | Run Once for Each Item | item.json     | item.json
List of items | Run Once for All Items | items[...]    | item.json

