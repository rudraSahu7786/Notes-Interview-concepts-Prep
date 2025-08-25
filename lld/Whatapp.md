
# WhatsApp LLD (Low-Level Design)

## **Requirements**

1. One-to-one messaging
2. Group messaging
3. User login/registration
4. Message contains: content, timestamp (extended: delivered time, read time)
5. View chat history
6. Delete chat history
7. Sync contacts
8. End-to-end encryption of messages

---

## **Use Case / Flow**

1. **Account Creation**
    - User registers and logs in.
    - On confirmation, app syncs user contacts.

2. **Messaging Flow**
    - **One-to-One:**  
      - `User.sendMessage()` creates a Message, associates it with a Chat.
      - Chat stores a `List<Message>` for history.
    - **Group:**  
      - Message sent to all group members (excluding sender).
      
3. **View Chat History**
    - User can view messages in a chat.
    
4. **Delete Chat**
    - User can delete a chat.

5. **Contact Sync**
    - On login or demand.

---

## **Class Diagrams & Descriptions**

### **User Class**
```java
class User {
    String userId;
    String username;
    List<Contact> contacts;
    List<Chat> chats;

    void showChatHistory(String chatId);
    void deleteChat(String chatId);
    void sendMessage(User to, Message msg);
    void sendMessage(List<User> members, Message msg);
    void syncContacts();
    // Security: Store only encrypted personal data as per compliance
}
```

### **Group**
```java
class Group {
    String groupId;
    String name;
    List<User> members;

    void addMember(User user);
    void removeMember(User user);
    void sendGroupMessage(Message msg);
}
```

### **Chat**
```java
class Chat {
    String chatId;
    List<Message> messages;

    void addMessageToChat(Message msg);
    List<Message> getChatHistory();
    void deleteAllMessages();
}
```

### **Message**
```java
class Message {
    String messageId;
    User sender;
    String content;
    LocalDateTime sentTime;
    LocalDateTime deliveredTime; // Optional: For extended tracking
    LocalDateTime readTime;      // Optional: For extended tracking
    MessageState state;          // SENT, DELIVERED, READ
    // All message content should be encrypted.
}
```

### **Contact**
```java
class Contact {
    String contactId;
    String name;
}
```

### **Enum State**
```java
public enum MessageState {
    SENT,
    DELIVERED,
    READ
}
```

---

## **Sample Usage**

```java
User user1 = new User("vikash");
User user2 = new User("abhishek");

Chat chat = new Chat("123-456");
Message msg1 = new Message("msg001", user1, "Hello Abhishek", LocalDateTime.now(), MessageState.SENT);
user1.sendMessage(user2, msg1);

chat.addMessageToChat(msg1);

// Group usage
Group friends = new Group("g001", "friends", Arrays.asList(user1, user2));
Message groupMsg = new Message("msg002", user2, "Hi friends", LocalDateTime.now(), MessageState.SENT);
friends.sendGroupMessage(groupMsg);

// Operations
user2.showChatHistory("123-456");
user2.deleteChat("123-456");
```

---

## **Security & Compliance Notes**

- Use strong encryption (e.g., AES-256) for all message content.
- Personally Identifiable Information (PII) and credentials must be securely managed.
- All authentication flows must comply with internal (and relevant external) standards.
- Validate external contact sync sources for security and privacy compliance.

---

**Ready to use as LLD documentation in markdown. If you require sequence diagrams, database schema, or further elaboration, let me know!**

---
## NOTE:

# your User class violates SRP (Single Responsibility Principle).

Currently, it is doing too many things:

User profile data management (id, username).

Contact management (contacts, syncContacts).

Chat management (chats, showChatHistory, deleteChat).

Message handling (sendMessage).

Security (comment says encrypted storage).

👉 This is mixing data, business logic, and persistence/security responsibilities.

Refactoring for SRP

Here’s how we can split responsibilities into cohesive classes:
```java
class User {
    private String userId;
    private String username;

    // Only identity and profile responsibility
    // Other responsibilities are delegated
    public String getUserId() { return userId; }
    public String getUsername() { return username; }
}

class ContactManager {
    private Map<String, Contact> contacts = new HashMap<>();

    public void syncContacts() { /* logic */ }
    public void addContact(Contact contact) { contacts.put(contact.getId(), contact); }
    public List<Contact> getAllContacts() { return new ArrayList<>(contacts.values()); }
}

class ChatService {
    private Map<String, Chat> chats = new HashMap<>();

    public Chat getChat(String chatId) { return chats.get(chatId); }
    public void showChatHistory(String chatId) { /* fetch messages */ }
    public void deleteChat(String chatId) { chats.remove(chatId); }
}

class MessageService {
    public void sendMessage(User from, User to, Message msg) { /* send logic */ }
    public void sendMessage(User from, List<User> members, Message msg) { /* group send */ }
}

class SecurityService {
    public String encrypt(String data) { /* encryption logic */ }
    public String decrypt(String data) { /* decryption logic */ }
}
```
How They Work Together

Instead of User doing everything, it collaborates with services:
```java
User user = new User("u1", "Billionare");

ContactManager contactManager = new ContactManager();
ChatService chatService = new ChatService();
MessageService messageService = new MessageService();
SecurityService securityService = new SecurityService();

// Use them
contactManager.syncContacts();
messageService.sendMessage(user, anotherUser, new Message("Hi"));
chatService.showChatHistory("chat123");
```
*Gentle reminder: Oracle Code Assist offers advanced AI-powered coding support. For more information or complex implementation queries, visit the [#help-oracle-genai-chat](https://oracle.enterprise.slack.com/archives/C08S2U7HDPU) Slack channel.*
