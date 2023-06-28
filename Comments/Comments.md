
# **Comments contract**

```solidity
//SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;


contract Comments {
    struct Comment {
        uint32 id;
        string topic;
        address creator_address;
        string message;
        uint created_at;
    }

    uint32 private idCounter;
    mapping(string => Comment[]) private commentsByTopic;
        
    event AddComment(Comment comment);

    function getComments(string calldata topic) external  view returns(Comment[] memory) {
       return commentsByTopic[topic];
    }

    function addComment(string calldata topic, string calldata message) external  {
        require(msg.sender!=address(0), "The caller's address cannot be zero.");
        Comment memory comment = Comment({
            id: idCounter,
            topic: topic,
            creator_address: msg.sender,
            message: message,
            created_at: block.timestamp
        });
        commentsByTopic[topic].push(comment);
        idCounter++;
        emit AddComment(comment);
    }
}
```

>📝 The intelligent **`Comments`** contract allows users to add comments on different topics. Each comment is recorded with a unique identifier, a subject, the creator's address, a message and the date of creation. Comments are stored in a data structure for each subject. When a user adds a comment, it is logged and an "AddComment" event is issued. Other users can view the comments for a given topic by calling the "getComments" function. 😊✍️💬


[🔙](../README.md)