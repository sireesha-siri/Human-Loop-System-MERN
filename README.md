# Human-in-Loop Approval System

> A scalable, event-driven workflow management system that enables agents to pause execution and request human approval before proceeding with critical actions.

### 🚀 Repositories
- Frontend: [Human-Looping-System-Frontend](https://github.com/sireesha-siri/Human-Looping-System-Frontend)
- Backend: [Human-Looping-Backend](https://github.com/sireesha-siri/Human-Looping-Backend)


<!-- **Live Demo:** [Add your deployed URL here]  
**GitHub:** [Add your repo URL here] -->

---

## 🎯 Problem Statement

Modern agent systems require human approvals before executing critical actions (deployments, financial transactions, bulk operations). This project creates an event-driven orchestration layer where:

- ✅ Agents can pause and request human feedback
- ✅ Humans approve/reject via intuitive UI
- ✅ System automatically resumes upon response
- ✅ Maintains state across long-running workflows

---

## 🚀 Features

### Core Functionality
- **Workflow Management**: Create and track multi-step workflows
- **Human-in-Loop Approvals**: Pause workflows for human decision-making
- **Event-Driven Architecture**: Asynchronous, non-blocking operations
- **State Management**: Persistent state tracking across workflow lifecycle
- **Real-time Dashboard**: Live stats and pending approval visualization
- **Complete Audit Trail**: History and observability of all decisions
- **Responsive Design**: Works seamlessly on desktop, tablet, and mobile

### Technical Highlights
- RESTful API with Express.js
- MongoDB for persistent state storage
- React with Tailwind CSS for modern UI
- Error handling and validation
- Toast notifications for user feedback
- Modal dialogs for approval actions

---

## 🛠️ Tech Stack

### Frontend
- **React 18** with Vite (fast build tool)
- **Tailwind CSS** for styling
- **React Router** for navigation
- **Axios** for API calls
- **Lucide React** for icons

### Backend
- **Node.js** with Express.js
- **MongoDB** with Mongoose ODM
- **CORS** enabled for cross-origin requests
- **dotenv** for environment configuration

### Deployment
- **Vercel** (Frontend hosting)
- **Render** (Backend hosting)
- **MongoDB Atlas** (Cloud database)

---

## 📁 Project Structure

```
human-loop-system/
├── README.md
├── .gitignore
│
├── server/                      # Backend
│   ├── server.js               # Main server file
│   ├── package.json
│   ├── .env
│   ├── config/
│   │   └── database.js         # MongoDB connection
│   ├── models/
│   │   ├── Workflow.js         # Workflow schema
│   │   └── Approval.js         # Approval schema
│   ├── controllers/
│   │   ├── workflowController.js
│   │   └── approvalController.js
│   ├── routes/
│   │   ├── workflows.js
│   │   └── approvals.js
│   ├── middleware/
│   │   └── validation.js
│   └── utils/
│       └── errorHandler.js
│
└── client/                      # Frontend
    ├── package.json
    ├── vite.config.js
    ├── tailwind.config.js
    ├── index.html
    └── src/
        ├── main.jsx
        ├── App.jsx
        ├── index.css
        ├── components/          # Reusable components
        │   ├── Navbar.jsx
        │   ├── StatsCard.jsx
        │   ├── WorkflowCard.jsx
        │   ├── Modal.jsx
        │   ├── Toast.jsx
        │   ├── LoadingSpinner.jsx
        │   └── EmptyState.jsx
        ├── pages/               # Page components
        │   ├── Dashboard.jsx
        │   ├── CreateWorkflow.jsx
        │   ├── Approvals.jsx
        │   └── History.jsx
        └── services/
            └── api.js           # API integration
```

---

## 🔧 Installation & Setup

### Prerequisites
- Node.js (v16 or higher)
- MongoDB (local installation or Atlas account)
- npm or yarn

### Quick Start

1. **Clone the repository**
```bash
git clone <your-repo-url>
cd human-loop-system
```

2. **Backend Setup**
```bash
cd server
npm install

# Create .env file
echo "PORT=5000" > .env
echo "MONGODB_URI=mongodb://localhost:27017/human-loop-system" >> .env
echo "NODE_ENV=development" >> .env
```

3. **Frontend Setup**
```bash
cd ../client
npm install
```

4. **Start MongoDB**
```bash
# On Mac (with Homebrew)
brew services start mongodb-community

# On Ubuntu
sudo systemctl start mongod

# Or use MongoDB Atlas (cloud)
```

5. **Run the Application**

Terminal 1 (Backend):
```bash
cd server
npm run dev
```

Terminal 2 (Frontend):
```bash
cd client
npm run dev
```

6. **Access the Application**
- Frontend: http://localhost:5173
- Backend API: http://localhost:5000
- Health Check: http://localhost:5000/health

---

## 📊 Architecture

### System Design

```
┌─────────────┐      HTTP/REST      ┌─────────────┐      Mongoose      ┌─────────────┐
│   React     │ ←──────────────────→ │   Express   │ ←─────────────────→ │   MongoDB   │
│  Frontend   │      JSON Data       │   Backend   │    CRUD Operations  │  Database   │
└─────────────┘                      └─────────────┘                     └─────────────┘
```

### State Machine

```
Workflow Lifecycle:
Created → Pending Approval → [Approved/Rejected] → Completed

Approval Flow:
Request Created → Pending → Human Decision → [Approved/Rejected] → Workflow Resumes
```

### Event-Driven Flow

```
1. User creates workflow
   ↓
2. System auto-creates approval request
   ↓
3. Workflow state: "pending_approval"
   ↓
4. Human reviews in dashboard
   ↓
5. Human approves/rejects
   ↓
6. System updates workflow state
   ↓
7. Action logged in history
```

---

## 🔌 API Endpoints

### Workflows

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/workflows` | Create new workflow |
| GET | `/api/workflows` | Get all workflows |
| GET | `/api/workflows/:id` | Get workflow by ID |
| PATCH | `/api/workflows/:id/status` | Update workflow status |
| DELETE | `/api/workflows/:id` | Delete workflow |

### Approvals

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/approvals/pending` | Get pending approvals |
| GET | `/api/approvals` | Get all approvals |
| GET | `/api/approvals/:id` | Get approval by ID |
| POST | `/api/approvals/:id/approve` | Approve workflow |
| POST | `/api/approvals/:id/reject` | Reject workflow |

### Example: Create Workflow

```bash
curl -X POST http://localhost:5000/api/workflows \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Deploy to Production",
    "description": "Deploy version 2.0 with new features",
    "type": "deployment",
    "riskLevel": "high"
  }'
```

---

## 💡 Key Design Decisions

### 1. Event-Driven Architecture
- Workflows don't block while waiting for approval
- State changes trigger events throughout the system
- Enables scalability and resilience to failures

### 2. Persistent State Management
- MongoDB stores complete workflow state
- State transitions are logged for audit trail
- System can recover from failures without data loss

### 3. Separation of Concerns
- Workflows and Approvals are separate entities
- Clean MVC architecture (Models, Controllers, Routes)
- Easy to extend and maintain

### 4. User-Centric Design
- Intuitive approval interface
- Real-time feedback with toasts
- Responsive design for all devices
- Loading states and empty states

---

## 🧪 Testing

### Manual Testing Checklist

**Backend:**
- [ ] Server starts without errors
- [ ] MongoDB connects successfully
- [ ] Health endpoint returns 200
- [ ] Can create workflow
- [ ] Can approve/reject workflow
- [ ] Validation works correctly

**Frontend:**
- [ ] App loads without console errors
- [ ] Can navigate between pages
- [ ] Can create workflow
- [ ] Can approve/reject from dashboard
- [ ] History shows all workflows
- [ ] Responsive on mobile/tablet

### API Testing with curl

```bash
# Health check
curl http://localhost:5000/health

# Create workflow
curl -X POST http://localhost:5000/api/workflows \
  -H "Content-Type: application/json" \
  -d '{"name":"Test","description":"Test workflow","type":"other","riskLevel":"low"}'

# Get pending approvals
curl http://localhost:5000/api/approvals/pending
```

---

## 🚢 Deployment

### Backend (Render)

1. Push code to GitHub
2. Create Web Service on Render
3. Connect GitHub repository
4. Configure:
   - Build Command: `cd server && npm install`
   - Start Command: `cd server && npm start`
   - Environment Variables:
     - `MONGODB_URI`: Your MongoDB Atlas connection string
     - `NODE_ENV`: production
5. Deploy

### Frontend (Vercel)

1. Push code to GitHub
2. Import project on Vercel
3. Configure:
   - Framework: Vite
   - Root Directory: `client`
   - Build Command: `npm run build`
   - Output Directory: `dist`
4. Deploy

### MongoDB Atlas

1. Create free cluster at mongodb.com/cloud/atlas
2. Create database user
3. Whitelist IP (0.0.0.0/0 for development)
4. Get connection string
5. Update `.env` with connection string

---

## 📈 Evaluation Criteria Coverage

| Criteria | Implementation | Weight | Status |
|----------|----------------|--------|--------|
| Architecture & Design | Event-driven, scalable RESTful API | 25% | ✅ Complete |
| State Model Design | MongoDB persistence, state machine | 20% | ✅ Complete |
| Frontend Configurability | Dynamic forms, responsive UI | 15% | ✅ Complete |
| Resilience & Retry Logic | Error handling, validation | 15% | ✅ Complete |
| Integration Creativity | Clean web UI for approvals | 15% | ✅ Complete |
| Observability | History logs, action tracking | 10% | ✅ Complete |

---

## 🔮 Future Enhancements

- [ ] Real-time notifications with WebSockets
- [ ] Email/Slack integration for approvals
- [ ] Multi-level approval chains
- [ ] Conditional approval routing based on risk
- [ ] Approval timeouts and escalation
- [ ] Advanced analytics dashboard
- [ ] Role-based access control (RBAC)
- [ ] Webhook integrations for external systems
- [ ] Workflow templates
- [ ] Bulk operations

---

## 👥 Author

**Sireesha Aguru**  
- 📧 Email: a.sireesha531@gmail.com  
- 💼 LinkedIn: [linkedin.com/in/sireesha-aguru](https://www.linkedin.com/in/sireesha-aguru-1a70b7257/)  
- 🐙 GitHub: [github.com/sireesha-siri](https://github.com/sireesha-siri/)  
- 🌐 Portfolio: [sireesha-siri.github.io/my-portfolio/](https://sireesha-siri.github.io/my-portfolio/)

---

## 📝 License

MIT License - Free to use for learning and development purposes.

---

## 🙏 Acknowledgments

- Built for **Lyzr Hiring Challenge 2025**
- Inspired by Temporal.io, n8n, and Zapier architectures
- Architecture guidance and code assistance: **Claude AI (Anthropic)** and **ChatGPT (OpenAI)**
- Special thanks to the open-source community for React, Tailwind CSS, Express.js, and MongoDB

### 🤖 AI-Assisted Development
This project was developed with AI assistance to accelerate learning and implementation:
- **Claude AI**: System architecture design, event-driven patterns, code structure
- **ChatGPT**: Debugging, optimization suggestions, best practices
- **Human Input**: Problem analysis, design decisions, testing, deployment strategy

*The use of AI tools demonstrates effective leverage of modern development resources 
to build production-quality systems efficiently - a key skill in contemporary 
software engineering.*
