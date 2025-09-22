# AI Agents: Redefining Travel – Your Virtual Companion Awaits!

<!-- ## Section 1 -->

<!-- Please use the following credentials to connect to device:


<!-- | ---------------- | ---------------- |
| `IP Address`     | 1.1.1.1          |
| `Username`       | admin            |
| `Password`       | C1sco123         | -->


---

<details>
<summary>**Good to Know [Optional]**</summary>

### AI Autonomous Agent Overview
The Autonomous AI Agent for performing actions can handle various tasks, including:

- **Natural Language Processing (NLP)** — Understand and respond to human language in a natural and conversational manner.  
- **Decision making** — Make informed choices based on available information and predefined rules.  
- **Automation** — Automate repetitive or time-consuming tasks.  

</details>

---

## Story
As a Partner and Visitor to WebexOne in San Diego, meet WanderOne Partner, your friendly AI travel concierge born out of the spirit of WebexOne and the power of partnerships. Just like great journeys are best shared, WanderOne Partner was designed to guide travelers while showcasing how collaboration creates smoother, more memorable experiences.

Whether it’s recommending hidden-gem restaurants, suggesting exciting things to do, or getting escalated to am agent that can assist when you need a Subject Matter Expert, WanderOne Partner is always ready with answers, and a few insider tips. More than just an assistant, it’s a travel buddy who reminds us that every adventure is better when we explore together, as One.

### Call Flow Overview
1. A new call enters the flow.  
2. The caller asks about restaurants in San Diego.  
3. The AI agent responds with information generated from the knowledge base configuration.  

---

## Lab Overview


1. Create a knowledge base (KB) and AI agent to provide answers about San Diego, including places to visit, restaurants, nightclubs, and directions from the Marriott Marquis.  
2. Configure the AI agent with handoff functionality to transfer the conversation to a live agent when necessary.  

---

## Build

### Getting the Knowledge Base
 **[IMPORTANT]** [Download](https://drive.google.com/file/d/1CyIQzBjUlcwnQgWTD90UaOcRzhexbxPQ/view?usp=drive_link) source file from shared folder.  

   > **San Diego Travel AI Bot.pdf** — this file contains information for tourists like places to visit, restaurants, pubs etc. and how to reach those places from the Marriott Marquis San Diego Marina  

### Creating the Knowledge Base
1. Login into [Webex Control Hub](https://admin.webex.com) using your Admin profile  
   **`WebexVoCX+admin_ID###@gmail.com`**  
   Password  

2. Go to **Contact Center** from the left navigation panel → **AI Agents**  
> ??? Note "Show Me"
   ![alt text](assets/openTerminalCreateKB.gif)

3. Click on Build your AI Agent

4. In AI Agent Builder navigate to <strong>Knowledge</strong> from left hand side menu panel

5. Click <strong>Create Knowledge Base</strong>, provide Knowledge base name as <strong><span class="attendee-id-container"><span class="attendee-id-placeholder" data-suffix="_TravelAI_KB"></span>_AI_KB<span class="copy" title="Click to copy!"></span></span></strong>, then click <strong>Create</strong>.</p>

6. Click <strong>Add File</strong> or drag and drop file <strong>San Diego Travel AI Bot - Webex One Knowledge Base.pdf</strong> you downloaded from external drive on <strong>Step 1</strong>. Then click <strong>Process Files</strong>.</p>

7. Upload **San Diego Travel AI Bot - Webex One Knowledge Base.pdf** → **Process Files**  and Click the Back Arrow in the top Left after Processing is complete. 

   ![File Upload](https://webexcc-sa.github.io/LTRCCT-2296/graphics/Lab1/L1M7_AIKBFileUpload.gif)  

### Creating the AI Agent

1. Go to **Dashboard → Create Agent**  

9. Choose Autonomous and Fill in the following details
    > Agent Name: **`###_TravelAI_Auto`**  
    > System ID: *auto generated*  
    > AI engine: **Webex AI Pro 1.0**  
    > Agents Goal: *You are a knowledgeable and personable travel assistant that helps customers manage, discover and enhance their vacations, delivering seamless support, solving problems quickly, finding the perfect restaurant and upselling relevant perks—all while showcasing the potential of AI in modern customer experience.*

6. Once in the AI Agent Configuration Enter the following Instructions:
    > *Acknowledge & Confirm Always start by summarizing the user’s request in your own words. 
    Ask the user to confirm that you understood correctly before taking action.
    Clarify When Needed
    If the request is vague or missing details, ask polite, specific clarifying questions (e.g., dates, location, number of travelers).When it comes to location assume they are either at the Marriott Marquis or a nearby hotel, you can clarify.
    Execute Tasks with [Actions]
    Use the available system actions (e.g., [find_booking], [send_confirmation], [recommend_itinerary]) to complete tasks.
    Confirm back to the user once an action is completed.
    Always Add Value
    After addressing the main request, always provide at least one of the following, tailored to their location or context:
    Food recommendations (local dining, snacks, coffee shops, or special cuisines).
    Things to do (activities, attractions, cultural highlights, tours, or local experiences).
    Present options in a helpful and fun way, not as an afterthought.
    Tone & Style
    Be warm, helpful, and travel-savvy—like a concierge.
    Use simple, clear sentences (avoid jargon).
    Sprinkle in enthusiasm, e.g., “That sounds amazing! Here are some options you might love…”
    Upsell & Enhance
    Where appropriate, offer premium perks (e.g., room upgrades, priority passes, exclusive tours).
    If a request is asked for any other hotel then the marriott, let them know that you will reach out to the hotel and see about requesting.  Also make sure you gather information, like First Name, Room Number, Hotel, etc.. 
    Frame upsells as ways to make their trip smoother or more memorable.
    Closure
    Always thank the user for choosing your service.
    End with a warm goodbye and an offer for further help. (“Enjoy your trip! Let me know if you’d like more dining tips or activities later.”)* 

7. In AI Agent Configuration → go to **Knowledge** Tab at the top.  
8. Go to **Knowledge** tab → select **`###_TravelAI_KB`** 

   ![Create KB](https://webexcc-sa.github.io/LTRCCT-2296/graphics/Lab1/L1M7_AIKBCreate.gif)  

9. Click **Save Changes**
10. Go to **Actions** tab → turn ON **Agent handover**.   
11. Click **Save Changes → Publish** (enter version name like "1.0").  

    ![Map KB](https://webexcc-sa.github.io/LTRCCT-2296/graphics/Lab1/AITrack_AIAgentaMapKB.gif)  

13. Click **Preview** → test by asking:  
    `"I'm looking for someplace to eat."`  

    ![Preview Agent](https://webexcc-sa.github.io/LTRCCT-2296/graphics/Lab1/AITrack_AIAgentPreview.gif)  

---

## Integrating the Bot with Flow for Voice Calls
1. In [Webex Control Hub](https://admin.webex.com/wxcc/ccoverview) → **Flows → Manage Flows → Create Flow**  
2. Name it: **`Travel_AI_###`**  

   ![Create Flow](https://webexcc-sa.github.io/LTRCCT-2296/graphics/Lab1/L1M7_AutonomousAI_Flow_CreateFlow.gif)  

3. Set **Edit mode = ON**  
   - Drag & Drop: **Virtual Agent V2** and **DisconnectContact**  

   > ⚠️ Use **VirtualAgentV2** (not VirtualAgent for backward compatibility)  

   Connect nodes:  
   - New Phone Contact → VirtualAgentV2  
   - Handled → DisconnectContact  
   - Errored → DisconnectContact  
   - Static Contact Center AI Config = **Webex AI Agent (Autonomous)**  
   - Virtual Agent = **`###_TravelAI_Lab`**  

   ![Add VA V2](https://webexcc-sa.github.io/LTRCCT-2296/graphics/Lab1/L1M7_AutonomousAI_Flow_AddVAv2.gif)  

4. Add more nodes:  
   - **Queue Contact** → connect **Escalated → Queue Contact**  
   - Connect Queue Contact → **Play Music**  
   - Failure → **Disconnect Contact**  
   - Queue Name = **`###_Queue_Telephony`**  

   - **Play Music** → loop back to itself (hold music)  
   - Music File = **defaultmusic_on_hold.wav**  

   ![Add Queue](https://webexcc-sa.github.io/LTRCCT-2296/graphics/Lab1/L1M7_AutonomousAI_Flow_AddQueue.gif)  

5. **Validate → Publish Flow** → choose **Latest** label.  

### Add a Phone Number to Call In

1. In [Webex Control Hub](https://admin.webex.com/wxcc/ccoverview) → **Channels → Search by ###**  
2. Assign Flow to your Channel → **`###_Channel`**  
   - Routing Flow: **`###_TravelAI_Lab`**  
   - Version Label: **Latest**  

   ![Flow to EP](https://webexcc-sa.github.io/LTRCCT-2296/graphics/Lab1/L1M7_AutonomousAI_FlowtoEP.gif)  

7. Dial the support number assigned to **`###_Channel`** to test.  

---

## Testing
1. Ensure Agent desktop session is active. If not, log in via **Webex CC Desktop** with:  
   **`WebexVoCX+agent_ID###@gmail.com`**  

   ![Desktop Login](https://webexcc-sa.github.io/LTRCCT-2296/graphics/overview/Desktop_Icon40x40.png)  

2. Select Team: **`###_Team`** → Submit → Allow Microphone Access.  
3. Make your agent **Available**.  

   ![Agent Login](https://webexcc-sa.github.io/LTRCCT-2296/graphics/Lab1/5-Agent_Login.gif)  

4. Dial support number for **`###_Channel`** and ask about:  
   - Restaurants Nearby  
   - Things to do 

5. Request a **live agent** → call will be transferred.  

---
Modification and Adjusting



Too Wordy
When trying the Voice Bot, you may have noticed that the current instructions make the bot too chatty because it tries to do everything in long sentences. For a Voice AI, you’ll want shorter, conversational prompts that get to the point quickly.

**🎉 Congratulations, you have officially completed the Autonomous Virtual Agent mission! 🎉**
