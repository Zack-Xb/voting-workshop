# \#4. Create and Test Your Voting Program


Here we will create a voting program of our own.

## Create a voting contract

For this demo, our desired features are following:

- Propose Function: Ability to propose a new proposal to be voted on.
- Issue Voting Ticket Function: Ability to issue a ticket to an address so they can vote.
- Vote Function: Ability for ticket holder to vote for or against the proposal.

## First we shall define all Structs, Records and Mappings needed.

### Step 1: define your Proposal Informations struct

```leo
 struct ProposalInfo {
        title: field,
        content: field,
        proposer: address,
    }
```

### Step 2: define your Proposal record and mapping for Proposal storage

Define a Proposal record with an `owner`, `id` and `info`.

```leo
   record Proposal {
        owner: address,
        id: field,
        info: ProposalInfo,
    }
```

```leo
mapping proposals: field => ProposalInfo;
```

### Step 3: Define Ticket record to store private votes.

The `pid` data field represents the proposal id.

```leo
 record Ticket {
        owner: address,
        pid: field,
    }
```

### Step 4: Define mappings to store the number of tickets issues, agree_votes and disagree_votes

```leo
   mapping tickets: field => u64;
   mapping agree_votes: field => u64;
   mapping disagree_votes: field => u64;
```

## Time for Transitions and Functions

### Step 5: Propose async Transition and Function

```leo
// Propose a new proposal to vote on.
    async transition propose(public info: ProposalInfo) -> (Proposal, Future) {
        // Authenticate proposer.
        assert_eq(self.caller, info.proposer);

        // Generate a new proposal id.
        let id: field = BHP256::hash_to_field(info.title);

        let proposal: Proposal = Proposal {
            owner: self.caller,
            id,
            info,
        };

        // Return a new record for the proposal.
        // Finalize the proposal id.
        return (proposal, propose_public_execution(id));
    }

    // Create a new proposal in the "tickets" mapping.
    async function propose_public_execution(id: field) {
        Mapping::set(tickets, id, 0u64);
    }
```

### Step 6: Ticket issuance async Transition and Function

```leo
// Create a new ticket to vote with.
    async transition new_ticket(
        public pid: field,
        public voter: address,
    ) -> (Ticket, Future) {

        let ticket: Ticket = Ticket {
            owner: voter,
            pid,
        };
        // Finalize the proposal id for the ticket.
        return (ticket, new_ticket_pub_exec(pid));
    }

    async function new_ticket_pub_exec(pid: field) {
        let current: u64 = Mapping::get_or_use(tickets, pid, 0u64);
        Mapping::set(tickets, pid, current + 1u64);
    }
```
### Step 7: Voting async Transition and Function

```leo
// Vote privately
    async transition vote(ticket: Ticket, agree: bool) -> Future {
        // Finalize this vote.
        return vote_pub_exec(ticket.pid, agree);
    }

    async function vote_public_execution(pid: field, agree: bool) {
        if agree {
            // Publicly increment the number of agree votes.
            let current: u64 = Mapping::get_or_use(agree_votes, pid, 0u64);
            Mapping::set(agree_votes, pid, current + 1u64);
        } else {
            // Publicly increment the number of disagree votes.
            let current: u64 = Mapping::get_or_use(disagree_votes, pid, 0u64);
            Mapping::set(disagree_votes, pid, current + 1u64);
        }
    }
```

## Test Program

### Test Mint function

First Test Mint Function.
```bash
leo run mint <owner address> 100u64
```

The output should be a record of new 100 tokens.

### Test Transfer function

Let's propose a new ballot. Take on the role of the proposer and run the propose transition function. We've provided the necessary information as inputs to the propose function.

echo '
NETWORK=testnet3
PRIVATE_KEY=APrivateKey1zkp8wKHF9zFX1j4YJrK3JhxtyKDmPbRu9LrnEW8Ki56UQ3G
' > .env

leo run propose '{
  title: 2077160157502449938194577302446444field,
  content: 1452374294790018907888397545906607852827800436field,
  proposer: aleo1rfez44epy0m7nv4pskvjy6vex64tnt0xy90fyhrg49cwe0t9ws8sh6nhhr
}'
