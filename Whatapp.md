Whatsapp LLD:
------------

Whatsapp

Saturday, 23 August 2025
10:12 PM

Requirements:

	1. One to one messaging
	2. Group messaging
	3. User can login or register
	4. Message should contain content + timestamp(extended as +delivered time+ read time)
	5. View chat history
	6. Delete chat history
	7. Sync contacts
	8. Encryption of msgs

 

Use case/flow Diagram:

	1. User creates account
	After user confirms app syncs contact
	2. User send message to another user 
		a. User.sendMesssage()-> create Message -> stored in the Chat 
		b. List<Message> msgList
	
	3. One to one messaging-> Message goes to the other user
	4. Group message-. Msg goes to all members of the Group
	5. User can view chats
	6. User can delete a Chat

Main classes
	- 
User class: 
	-String userId
	-String username
	-List<Contact> contacts
	-List<Chats> chats
	+showChatHistory(String chatId)
	+deleteChat(string chatid)
	
GroupCreate: extend User
	+createGroup(String name,List<User> )

MessageSend: extend User
	+
	+sendMessage(User to, Message msg)
	+sendMessage(List<User> members, Message ,msg)
	
CombinedUser extend GroupCreate,MessageSend:
	…..
	
Chat class:
	- String chatId
	- List<Message> msgs
	+addMessageToChat(Message msg)

Message:
	-String msgid
	-User sender
	-String content
	- -Boolean state
	- -LocalDateTime sendtime
	- -Enum state

GroupChat class;
	- String id;
	- List<User> members

	
	
Contact class; 

-List<String> contacts

Public Enum State{
SENT,
DELIVERED,
READ
}


--------------------------------------------


CombinedUser user1= new User("Vikash");
CombinedUser user2= new User('abhishek');
Chat chat= new Chat("123-456");

Message msg1= new Message("Hello Abhishek', time, false);

User1.sendMessage(user2, msg1, State.SENT);//sent msg to vikash

User2.readMessage(msg, time, State.READ)

Chat.addMessageToChat(msg1);

/// create group
User2.createGroup("friends",new ArrayList(user1,user1));

///user2.sendMessage(new ArrayList(user1,user1), "hi freinds");

User2.showChatHistory(123-456)
User2.deleteChat(123-456)
