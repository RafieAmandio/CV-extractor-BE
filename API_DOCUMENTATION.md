# CV Data Extractor API Documentation

## Chat Endpoints

### 1. Chat with AI
Interact with the AI assistant to analyze CV data and find job matches.

**Endpoint:** `POST /api/cv/chat`

**Request Body:**
```json
{
  "message": "Find CVs with Python experience",
  "cvId": "optional_cv_id"  // Optional: Focus conversation on specific CV
}
```

**Response:**
```json
{
  "success": true,
  "message": "Chat processed successfully",
  "data": {
    "response": "I found 3 CVs with Python experience...",
    "functionResult": {
      "name": "searchCVs",
      "arguments": {
        "query": "Python",
        "limit": 10
      },
      "result": [
        // Array of matching CVs
      ]
    }
  }
}
```

**Example Usage:**
```bash
curl -X POST http://localhost:3000/api/cv/chat \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Find CVs with Python experience",
    "cvId": "123456789"
  }'
```

**Available AI Functions:**
- `searchCVs`: Search for CVs based on specific criteria
- `getCVDetails`: Get detailed information about a specific CV
- `getJobMatches`: Get job matches for a specific CV

### 2. Get Chat History
Retrieve the history of chat interactions with pagination and filtering options.

**Endpoint:** `GET /api/cv/chat/history`

**Query Parameters:**
- `page` (number, default: 1): Page number for pagination
- `limit` (number, default: 20): Number of items per page
- `cvId` (string, optional): Filter by specific CV ID
- `startDate` (string, optional): Filter by start date (ISO format)
- `endDate` (string, optional): Filter by end date (ISO format)

**Response:**
```json
{
  "success": true,
  "message": "Chat history retrieved successfully",
  "data": {
    "history": [
      {
        "message": "Find CVs with Python experience",
        "response": "I found 3 CVs with Python experience...",
        "cvId": {
          "_id": "cv_id",
          "fileName": "example.pdf",
          "personalInfo": {
            "name": "John Doe"
          }
        },
        "functionCalls": [
          {
            "name": "searchCVs",
            "arguments": {
              "query": "Python",
              "limit": 10
            },
            "result": [
              // Array of matching CVs
            ]
          }
        ],
        "createdAt": "2024-03-20T10:30:00Z"
      }
    ],
    "pagination": {
      "total": 50,
      "page": 1,
      "limit": 20,
      "totalPages": 3,
      "hasNext": true,
      "hasPrev": false
    }
  }
}
```

**Example Usage:**
```bash
# Get all history (paginated)
curl "http://localhost:3000/api/cv/chat/history?page=1&limit=20"

# Get history for a specific CV
curl "http://localhost:3000/api/cv/chat/history?cvId=123456789"

# Get history within a date range
curl "http://localhost:3000/api/cv/chat/history?startDate=2024-01-01&endDate=2024-03-20"

# Combine filters
curl "http://localhost:3000/api/cv/chat/history?cvId=123456789&startDate=2024-01-01&endDate=2024-03-20&page=1&limit=10"
```

**Error Responses:**
```json
// Invalid request
{
  "success": false,
  "message": "Message is required"
}

// CV not found
{
  "success": false,
  "message": "CV not found"
}

// Server error
{
  "success": false,
  "message": "Internal server error"
}
```

**Notes:**
1. The chat endpoint automatically stores all interactions in the history
2. Function calls and their results are preserved in the history
3. The history endpoint supports flexible filtering and pagination
4. All dates should be in ISO format (YYYY-MM-DD)
5. The response includes basic CV information when available
6. The pagination object helps with implementing infinite scroll or pagination UI 