# New 'signalling' for unRupt

The current signalling model sufficed for the 1-2-1 simple demo we designed it for.

Now however there is an aspiration to have multiple a long running conversations
each between two parties.

In this model Brian has a bookmark for a conversation with Tim, and one for a conversation with David.
These links should bring him to a page that is ready for use, without the need for
QR codes or 'accept' buttons (which are only needed for the first call in a given conversation)

For this to work we need a unique conversation ID which is usable by both sides.
We also need to create id's for the participants that are static - but are not shared across 
conversations. 
(At the moment a given user/browser combo will always register with the rendezvous server with the same id
This means that if one opens multiple tabs it becomes pot-luck which one gets messages from the 
rendezvous server)

## Proposal
1) unruptid becomes a conversation id convoId
2) each side has a unique instanceId 
3) each side stores the other's instanceId in localStorage[conversationId]
4) each side uses ${instanceId}:${conversationId} to register with the rendezvous service
5) the page comes up with the pause button in 'pause' for both sides
6) the initiator is the first one to un-pause
7) the recipient also unpauses to accept a call ? 
Or it starts itself ? (i.e. having the page open - and visible? - implies acceptance.)

### How do we bootstrap a call?

* initiator is the one with no convoId on the url
This creates a new conversationId and reloads the page.

* The QR shows if localStorage[conversationId] is empty.
* the QR contains farInstanceId=${instanceId} & convoId=${conversationId} 
* acceptor has unruptid _and_ fid on the url
* initiator is sent "accept" message over ws
* this pops the QR down
* acceptor is sent "accepted" message over ws in reply.
* they both set localStorage[conversationId] =farInstanceId
* and reload page as ?convoId=${conversationId} 


### How does an established convo start?
* assume both are on ?convoId=${conversationId}
* both users are 'paused'
* first user presses 'unpause'
* sends webrtc offer
* second receives offer and sends answer (but does not enable mic/speaker yet)
* when/if second unpauses audio is enabled.




