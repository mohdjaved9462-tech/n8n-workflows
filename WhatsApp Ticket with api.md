# **WhatsApp Ticket with api**

{

&nbsp; "nodes": \[

&nbsp;   {

&nbsp;     "parameters": {

&nbsp;       "httpMethod": "POST",

&nbsp;       "path": "WhatsApp Ticket with Api",

&nbsp;       "responseMode": "responseNode",

&nbsp;       "options": {}

&nbsp;     },

&nbsp;     "id": "4737074d-9ada-47a5-8e0f-81f25167c4a3",

&nbsp;     "name": "Received message WhatsApp",

&nbsp;     "type": "n8n-nodes-base.webhook",

&nbsp;     "typeVersion": 1,

&nbsp;     "position": \[

&nbsp;       -2992,

&nbsp;       -1760

&nbsp;     ],

&nbsp;     "webhookId": "7281248c-e294-4b51-b151-30609b324a88"

&nbsp;   },

&nbsp;   {

&nbsp;     "parameters": {

&nbsp;       "conditions": {

&nbsp;         "options": {

&nbsp;           "caseSensitive": true,

&nbsp;           "leftValue": "",

&nbsp;           "typeValidation": "loose",

&nbsp;           "version": 3

&nbsp;         },

&nbsp;         "conditions": \[

&nbsp;           {

&nbsp;             "id": "b51c084f-60d6-41d7-b088-7219813dbefa",

&nbsp;             "leftValue": "={{ $json.type }}",

&nbsp;             "rightValue": "CREATE",

&nbsp;             "operator": {

&nbsp;               "type": "string",

&nbsp;               "operation": "equals"

&nbsp;             }

&nbsp;           }

&nbsp;         ],

&nbsp;         "combinator": "and"

&nbsp;       },

&nbsp;       "looseTypeValidation": true,

&nbsp;       "options": {}

&nbsp;     },

&nbsp;     "type": "n8n-nodes-base.if",

&nbsp;     "typeVersion": 2.3,

&nbsp;     "position": \[

&nbsp;       -2400,

&nbsp;       -1760

&nbsp;     ],

&nbsp;     "id": "33951805-9225-446d-a0d8-7d53060c884b",

&nbsp;     "name": "If"

&nbsp;   },

&nbsp;   {

&nbsp;     "parameters": {

&nbsp;       "respondWith": "allIncomingItems",

&nbsp;       "options": {}

&nbsp;     },

&nbsp;     "type": "n8n-nodes-base.respondToWebhook",

&nbsp;     "typeVersion": 1.5,

&nbsp;     "position": \[

&nbsp;       -1632,

&nbsp;       -1744

&nbsp;     ],

&nbsp;     "id": "de8658cf-9e27-4ca8-bf59-efeced0f2f61",

&nbsp;     "name": "Respond to WhatsApp"

&nbsp;   },

&nbsp;   {

&nbsp;     "parameters": {

&nbsp;       "respondWith": "allIncomingItems",

&nbsp;       "options": {}

&nbsp;     },

&nbsp;     "type": "n8n-nodes-base.respondToWebhook",

&nbsp;     "typeVersion": 1.5,

&nbsp;     "position": \[

&nbsp;       -2000,

&nbsp;       -1584

&nbsp;     ],

&nbsp;     "id": "9dfd5a0d-751c-4283-91ff-9490372cbcbd",

&nbsp;     "name": "Respond to WhatsApp-1"

&nbsp;   },

&nbsp;   {

&nbsp;     "parameters": {

&nbsp;       "method": "POST",

&nbsp;       "url": "http://query.invadertech.com/mobile/QueryService.svc/AddTicketWA",

&nbsp;       "sendBody": true,

&nbsp;       "specifyBody": "json",

&nbsp;       "jsonBody": "={\\n  \\"sessionId\\": {{ JSON.stringify($('Received message WhatsApp').item.json.body.sessionId) }},\\n  \\"displayName\\": {{ JSON.stringify($('Received message WhatsApp').item.json.body.displayName) }},\\n  \\"isGroup\\": {{ $('Received message WhatsApp').item.json.body.isGroup || false }},\\n  \\"sender\\": {{ JSON.stringify($('Received message WhatsApp').item.json.body.sender) }},\\n  \\"replyTo\\": {{ JSON.stringify($('Received message WhatsApp').item.json.body.replyTo) }},\\n  \\"groupId\\": {{ JSON.stringify($('Received message WhatsApp').item.json.body.groupId || null) }},\\n  \\"groupName\\": {{ JSON.stringify($('Received message WhatsApp').item.json.body.groupName || null) }},\\n  \\"timestamp\\": {{ JSON.stringify($('Received message WhatsApp').item.json.body.timestamp) }},\\n  \\"type\\": {{ JSON.stringify($('Received message WhatsApp').item.json.body.type) }},\\n  \\"message\\": {{ JSON.stringify($('Received message WhatsApp').item.json.body.message || \\"\\") }},\\n  \\"media\\": null,\\n  \\"mimeType\\": {{ JSON.stringify($json.mediaUrl?.mimeType || null) }},\\n  \\"fileName\\": {{ JSON.stringify($json.mediaUrl?.fileName || null) }}\\n}",

&nbsp;       "options": {}

&nbsp;     },

&nbsp;     "type": "n8n-nodes-base.httpRequest",

&nbsp;     "typeVersion": 4.3,

&nbsp;     "position": \[

&nbsp;       -2000,

&nbsp;       -1920

&nbsp;     ],

&nbsp;     "id": "f37b0e1a-01e7-4aec-adc4-9263b2c79377",

&nbsp;     "name": "Query Manager-Api"

&nbsp;   },

&nbsp;   {

&nbsp;     "parameters": {

&nbsp;       "language": "javascript",

&nbsp;       "jsCode": "const data = $input.first()?.json || {};\\nconst body = data.body || data || {};\\n\\n\\n// =====================\\n// READ MESSAGE\\n// =====================\\nconst rawText =\\n  body.message ??\\n  body.text ??\\n  body.chatInput ??\\n  \\"\\";\\n\\nconst message = String(rawText).trim().toLowerCase();\\n\\n// =====================\\n// MEDIA DETECTION\\n// =====================\\nconst hasMedia =\\n  body.type === \\"image\\" ||\\n  body.type === \\"video\\" ||\\n  body.type === \\"audio\\" ||\\n  body.type === \\"document\\" ||\\n  body.type === \\"pdf\\" ||\\n  body.media !== null ||\\n  body.mediaUrl !== undefined;\\n\\nconst mediaType = body.type || \\"unknown\\";\\nconst mediaUrl = body.mediaUrl || body.media || null;\\nconst fileName = body.fileName || null;\\n\\n// =====================\\n// GREETING WORDS\\n// =====================\\nconst greetings = \[\\n  \\"hi\\", \\"hello\\", \\"hey\\", \\"hii\\",\\n  \\"how are you\\", \\"good morning\\",\\n  \\"good evening\\", \\"good afternoon\\"\\n];\\n\\n// =====================\\n// THANK YOU WORDS\\n// =====================\\nconst thanksWords = \[\\n  \\"thanks\\", \\"thank you\\", \\"thx\\", \\"ty\\",\\n  \\"ok\\", \\"okay\\", \\"fine\\", \\"cool\\",\\n  \\"nice\\", \\"great\\", \\"👍\\", \\"🙏\\"\\n];\\n\\n// =====================\\n// RANDOM MEDIA REPLIES (ENGLISH)\\n// =====================\\nconst randomMediaReplies = \[\\n  \\"📸 Thank you for sharing the image. We couldn’t clearly identify the issue from the photo. Could you please describe the problem you’re facing so we can assist you better?\\",\\n  \\"👀 We’ve received your image, but the issue is not clearly visible. Kindly provide a short description of the problem for faster support.\\",\\n  \\"👍 Image received successfully. To help you accurately, please let us know the exact issue you are experiencing.\\",\\n  \\"📎 Thanks for the media. We are unable to determine the concern from the file alone. Please share a brief explanation of the problem.\\",\\n  \\"📸 We’ve received your image, but we couldn’t identify a specific issue. Could you please share a brief description of the problem so we can assist you quickly?\\"\\n];\\n\\n// =====================\\n// PROBLEM KEYWORDS (EXTENDED)\\n// =====================\\nconst problemKeywords = \[\\n  \\"problem\\", \\"issue\\", \\"error\\", \\"not found\\", \\"404\\", \\"not working\\", \\"failed\\",\\n  \\"unable\\", \\"can't\\", \\"cannot\\", \\"complaint\\", \\"help\\",\\n  \\"support\\", \\"bug\\", \\"crash\\", \\"slow\\", \\"down\\",\\n\\n  \\"app not opening\\", \\"app crash\\", \\"system error\\",\\n  \\"page not loading\\", \\"screen stuck\\", \\"frozen\\",\\n  \\"server error\\", \\"timeout\\", \\"something went wrong\\",\\n\\n  \\"login issue\\", \\"login failed\\", \\"unable to login\\",\\n  \\"account issue\\", \\"account blocked\\", \\"account suspended\\",\\n  \\"password issue\\", \\"forgot password\\", \\"otp issue\\",\\n\\n  \\"payment issue\\", \\"payment failed\\", \\"payment pending\\",\\n  \\"amount deducted\\", \\"double payment\\", \\"refund\\",\\n  \\"refund pending\\", \\"billing issue\\", \\"transaction failed\\",\\n\\n  \\"internet issue\\", \\"network issue\\", \\"no internet\\",\\n  \\"connection problem\\", \\"slow internet\\",\\n\\n  \\"file not uploading\\", \\"upload failed\\",\\n  \\"download issue\\", \\"file missing\\", \\"file corrupted\\",\\n\\n  \\"slow response\\", \\"lag\\", \\"delay\\", \\"taking too long\\",\\n  \\"technical issue\\", \\"service down\\", \\"not responding\\",\\n  \\"error message\\", \\"issue persists\\"\\n];\\n\\n// =====================\\n// INDIA TIME\\n// =====================\\nconst now = new Date();\\nconst createdAt = now.toLocaleString(\\"en-IN\\", {\\n  timeZone: \\"Asia/Kolkata\\",\\n  day: \\"2-digit\\",\\n  month: \\"short\\",\\n  year: \\"numeric\\",\\n  hour: \\"2-digit\\",\\n  minute: \\"2-digit\\",\\n  second: \\"2-digit\\",\\n  hour12: true,\\n});\\n\\n// =====================\\n// TICKET NUMBER\\n// =====================\\nconst yyyy = now.getFullYear();\\nconst mm = String(now.getMonth() + 1).padStart(2, \\"0\\");\\nconst dd = String(now.getDate()).padStart(2, \\"0\\");\\nconst hh = String(now.getHours()).padStart(2, \\"0\\");\\nconst mi = String(now.getMinutes()).padStart(2, \\"0\\");\\nconst ss = String(now.getSeconds()).padStart(2, \\"0\\");\\n\\nconst ticketNo = `TK\_NO-${yyyy}${mm}${dd}-${hh}${mi}${ss}`;\\n\\n// =====================\\n// COMMON RESPONSE FORMAT\\n// =====================\\nfunction response(data) {\\n  return \[{\\n    json: {\\n      reply: true,\\n      type: data.type || null,\\n      ticket: data.ticket || null,\\n      ticketStatus: data.ticketStatus || null,\\n      userIssue: data.userIssue || null,\\n      mediaType: data.mediaType || null,\\n      mediaUrl: data.mediaUrl || null,\\n      fileName: data.fileName || null,\\n      createdAt: data.createdAt || null,\\n      message: data.message || null\\n    }\\n  }];\\n}\\n\\n// =====================\\n// STATUS CHECK\\n// =====================\\nconst ticketMatch = message.match(/tk\_no-\\\\d+/i);\\nif (ticketMatch) {\\n  return response({\\n    type: \\"STATUS\\",\\n    ticket: ticketMatch\[0].toUpperCase(),\\n    message: `🔍 Checking status for ticket ${ticketMatch\[0].toUpperCase()}`\\n  });\\n}\\n\\n// =====================\\n// GREETING CHECK\\n// =====================\\nconst isGreeting = greetings.some(g =>\\n  message === g || message.startsWith(g)\\n);\\n\\nif (isGreeting) {\\n  return response({\\n    type: \\"GREETING\\",\\n    message: \\"👋 Hello! Please tell me your issue so I can assist you.\\"\\n  });\\n}\\n\\n// =====================\\n// MEDIA HANDLING\\n// =====================\\nif (hasMedia) {\\n\\n  const hasProblemInMedia = problemKeywords.some(k =>\\n    message.includes(k)\\n  );\\n\\n  // ✅ Media + Problem → Create Ticket\\n  if (hasProblemInMedia \&\& message.length > 3) {\\n    return response({\\n      type: \\"CREATE\\",\\n      ticket: ticketNo,\\n      ticketStatus: \\"OPEN\\",\\n      userIssue: message,\\n      mediaType,\\n      mediaUrl,\\n      fileName,\\n      createdAt,\\n      message:\\n        `📎 Media received successfully.\\\\n` +\\n        `✅ Your ticket has been created.\\\\n` +\\n        `🎫 Ticket No: ${ticketNo}\\\\n` +\\n        `🗂️ File Type: ${mediaType}\\\\n` +\\n        `🕒 Time: ${createdAt}\\\\n` +\\n        `Our support team will review it shortly.`\\n    });\\n  }\\n\\n  // ❌ Media only → Random reply\\n  const randomReply =\\n    randomMediaReplies\[Math.floor(Math.random() \* randomMediaReplies.length)];\\n\\n  return response({\\n    type: \\"MEDIA\_ONLY\\",\\n    mediaType,\\n    mediaUrl,\\n    fileName,\\n    createdAt,\\n    message: randomReply\\n  });\\n}\\n\\n// =====================\\n// TEXT PROBLEM CHECK\\n// =====================\\nconst isProblem = problemKeywords.some(k =>\\n  message.includes(k)\\n);\\n\\nif (isProblem \&\& message.length > 3) {\\n  return response({\\n    type: \\"CREATE\\",\\n    ticket: ticketNo,\\n    ticketStatus: \\"OPEN\\",\\n    userIssue: message,\\n    createdAt,\\n    message:\\n      `✅ Your ticket has been created successfully.\\\\n` +\\n      `🎫 Ticket No: ${ticketNo}\\\\n` +\\n      `🕒 Time: ${createdAt}\\\\n` +\\n      `Our team will contact you shortly.`\\n  });\\n}\\n\\n// =====================\\n// THANK YOU CHECK\\n// =====================\\nconst isThanks = thanksWords.some(w =>\\n  message.includes(w)\\n);\\n\\nif (isThanks) {\\n  return response({\\n    type: \\"THANKS\\",\\n    message: \\"😊 You’re welcome! Let me know if you need any further help.\\"\\n  });\\n}\\n\\n// =====================\\n// UNKNOWN MESSAGE\\n// =====================\\nreturn response({\\n  type: \\"UNKNOWN\\",\\n  message:\\n    \\"👋 Hello! Please describe your problem so I can create a support ticket.\\"\\n});"

&nbsp;     },

&nbsp;     "id": "93b5ac66-c29a-42a4-92f0-345d31ea85a8",

&nbsp;     "name": "Generate Ticket(JavaScript)",

&nbsp;     "type": "n8n-nodes-base.code",

&nbsp;     "typeVersion": 1,

&nbsp;     "position": \[

&nbsp;       -2704,

&nbsp;       -1760

&nbsp;     ]

&nbsp;   }

&nbsp; ],

&nbsp; "connections": {

&nbsp;   "Received message WhatsApp": {

&nbsp;     "main": \[

&nbsp;       \[

&nbsp;         {

&nbsp;           "node": "Generate Ticket(JavaScript)",

&nbsp;           "type": "main",

&nbsp;           "index": 0

&nbsp;         }

&nbsp;       ]

&nbsp;     ]

&nbsp;   },

&nbsp;   "If": {

&nbsp;     "main": \[

&nbsp;       \[

&nbsp;         {

&nbsp;           "node": "Query Manager-Api",

&nbsp;           "type": "main",

&nbsp;           "index": 0

&nbsp;         }

&nbsp;       ],

&nbsp;       \[

&nbsp;         {

&nbsp;           "node": "Respond to WhatsApp-1",

&nbsp;           "type": "main",

&nbsp;           "index": 0

&nbsp;         }

&nbsp;       ]

&nbsp;     ]

&nbsp;   },

&nbsp;   "Query Manager-Api": {

&nbsp;     "main": \[

&nbsp;       \[

&nbsp;         {

&nbsp;           "node": "Respond to WhatsApp",

&nbsp;           "type": "main",

&nbsp;           "index": 0

&nbsp;         }

&nbsp;       ]

&nbsp;     ]

&nbsp;   },

&nbsp;   "Generate Ticket(JavaScript)": {

&nbsp;     "main": \[

&nbsp;       \[

&nbsp;         {

&nbsp;           "node": "If",

&nbsp;           "type": "main",

&nbsp;           "index": 0

&nbsp;         }

&nbsp;       ]

&nbsp;     ]

&nbsp;   }

&nbsp; },

&nbsp; "pinData": {},

&nbsp; "meta": {

&nbsp;   "templateCredsSetupCompleted": true,

&nbsp;   "instanceId": "fc8ea56a9f77461efdbaf4ab14000fda259d6eb0a0ad047cd98c8f84ded3bdb0"

&nbsp; }

}

