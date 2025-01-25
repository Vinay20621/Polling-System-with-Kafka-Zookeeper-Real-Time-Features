# Real-Time Polling System

Build a backend system that supports a high-concurrency polling feature using Kafka and
Zookeeper. The system should allow multiple users to interact with polls simultaneously,
ensure resiliency in case of failures, and include real-time poll updates using WebSockets
along with a leaderboard feature. The system must ensure no votes are lost, even in the
event of system failures.
## Features

- Authentication with sign-up and sign-in functionality
- Only authenticated users can create, update, and delete polls
- Only authenticated users can vote, and each user can vote only once in a poll
- Real-time vote tracking using WebSocket
- Event-driven architecture using Kafka
- RESTful API endpoints
- Leaderboard system with time-based filtering
- Comprehensive statistics and analytics
- Database persistence with PostgreSQL
- Scalable and maintainable codebase

## Prerequisites

- **Node.js** (v14 or higher)
- **PostgreSQL** (v12 or higher)
- **Apache Kafka**
- **npm** or **yarn**

## Installation

### 1. Clone the repository:

```bash
git clone [repository-url]
cd polling-system
```

### 2. Install dependencies:

```bash
npm install
```

### 3. Set up environment variables:

Create a `.env` file in the root directory and add the following configurations:

```bash
PORT=3000
DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=your_password
DB_NAME=polling_system
JWT_SECRET=your_jwt_secret
```

### 4. Set up PostgreSQL database:

```bash
CREATE DATABASE polling_system;
```

### 5. Start the Kafka server and create the required topics:

#### Start Zookeeper
To start the Zookeeper server, run the following command:

```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

#### Start Kafka
To start the Kafka, run the following command:
```bash
bin/kafka-server-start.sh config/server.properties
```

#### Create topic
```bash
bin/kafka-topics.sh --create --topic poll-votes --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
```



## API Endpoints

#### Authentication

POST /api/v1/auth/signUp
 - Sign up a new user

POST /api/v1/auth/signIn
 - Sign in an existing user and get a JWT token


#### Polls

POST /api/v1/polls/createPoll 
- Create a new poll (authenticated users only)

GET /api/v1/polls/getPoll/:id 
- Get poll by ID

GET /api/v1/polls/allpolls 
- List all polls

PUT /api/v1/polls/updatePoll/:id
 - Update a poll (authenticated users only)

DELETE /api/v1/polls/deletePoll/:id
 - Delete a poll (authenticated users only)

#### Votes

###### POST /api/v1/vote/:optionId

 - Cast a vote for an option

#### Leaderboard

GET  /api/v1/leaderboard/:pollId?
- Get a leaderboard for a specific poll

GET /api/v1/leaderboard/:pollId/option/:optionId
- Get the leaderboard for a specific option within a poll

## WebSocket Events

### Client -> Server

```bash
// Cast a vote
{
    type: 'vote',
    data: {
        userId: 'string',
        optionId: 'string'
    }
}
```

### Server -> Client

```bash
// Vote update
{
    type: 'vote_update',
    data: {
        optionId: 'string',
        newVoteCount: number
    }
}
```

## Running the Application

### Start the server:

```bash
npm start
```

### Test WebSocket connection (optional):

```bash
node src/testWebSocketClient.js
```

### Testing
To run the test WebSocket client:

```bash
node src/testWebSocketClient.js
```

## Error Handling
#### The application includes comprehensive error handling:

- Input validation

- Authentication errors (for protected routes)

- Database connection errors

- WebSocket connection errors

- Kafka producer/consumer errors

- Duplicate vote prevention

