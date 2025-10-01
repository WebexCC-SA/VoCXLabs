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

# Webex One AI Agent Lab

<!-- Configuration Form -->
<div style="padding: 1em; border: 1px solid #ccc; border-radius: 8px; margin-bottom: 1em;">
  <h3>🔧 Enter Your Assigned Number</h3>
  <label for="attendeeId">Attendee Number (101–180): </label>
  <input type="number" id="attendeeId" min="101" max="180" />
  <button onclick="saveAndUpdate()">Update Lab Guide</button>
  <button onclick="resetId()">Reset My ID</button>
</div>

<!-- Status Banner -->
<div id="statusBanner" style="
  display:none;
  opacity: 0;
  padding: 10px;
  margin-bottom: 1em;
  border-radius: 6px;
  font-weight: bold;
  transition: opacity 0.5s ease-in-out;
"></div>

---

## Lab Configuration

- **Inbound Channel Name:** ###_Channel  
- **Inbound Channel Phone Number:** <span id="channelPhone"></span>  
- **Queue Name:** ###_Queue 
- **Team Name:** ###_Team  
- **Admin/Agent Name:** WebexVoCX+admin_ID###@gmail.com
- **Password:** Webex123!

<script>
  // ID → phone mapping
const phoneMap = {
  101:"9162602144",
  102:"9162650493",
  103:"9163028535",
  104:"9162448537",
  105:"9162602392",
  106:"9162690328",
  107:"9162690642",
  108:"9162690670",
  109:"9162700449",
  110:"9162700453",
  111:"9162701348",
  112:"9162701592",
  113:"9162701659",
  114:"9163021569",
  115:"9163021824",
  116:"9163021836",
  117:"9163021837",
  118:"9163021846",
  119:"9163021863",
  120:"9163021918",
  121:"9163021921",
  122:"9163021934",
  123:"9163021995",
  124:"9163120098",
  125:"9163120316",
  126:"9163120586",
  127:"9163121265",
  128:"9164576247",
  129:"9164649757",
  130:"9164649945",
  131:"9165458966",
  132:"9165797842",
  133:"9166020131",
  134:"9166020357",
  135:"9166020395",
  136:"9166020423",
  137:"9166020529",
  138:"9166020659",
  139:"9166020981",
  140:"9166180651",
  141:"9166180956",
  142:"9166180996",
  143:"9166208473",
  144:"9166208664",
  145:"9166208966",
  146:"9166215282",
  147:"9166215326",
  148:"9166215476",
  149:"9166215606",
  150:"9166215628",
  151:"9166215931",
  152:"9166215974",
  153:"9166216049",
  154:"9166216091",
  155:"9166216291",
  156:"9166599038",
  157:"9166599060",
  158:"9166599066",
  159:"9166599073",
  160:"9166599083",
  161:"9166599087",
  162:"9166599089",
  163:"9166599103",
  164:"9166599104",
  165:"9166599691",
  166:"9166654795",
  167:"9167130228",
  168:"9167130234",
  169:"9167130271",
  170:"9167130323",
  171:"9167130424",
  172:"9167130513",
  173:"9167130692",
  174:"9167130699",
  175:"9167130725",
  176:"9167442983",
  177:"9167969809",
  178:"9168663904",
  179:"9168849040",
  180:"9168849162"
};


  // Replace all ### with attendee ID, except phone number
  function updateConfig(id){
    // Update phone
    document.getElementById("channelPhone").innerText = "+1" + phoneMap[id];

    // Scan entire body and replace ### with ID
    document.body.innerHTML = document.body.innerHTML.replace(/###/g, id);
  }

  function showBanner(message, type){
    const banner = document.getElementById("statusBanner");
    banner.innerText = message;
    banner.style.backgroundColor = type==="success"?"#d4edda":"#f8d7da";
    banner.style.color = type==="success"?"#155724":"#721c24";
    banner.style.border = type==="success"?"1px solid #c3e6cb":"1px solid #f5c6cb";

    banner.style.display = "block";
    banner.style.opacity = "0";
    setTimeout(()=>banner.style.opacity="1",10); // fade in
    setTimeout(()=>banner.style.opacity="0",4000); // fade out
    setTimeout(()=>banner.style.display="none",4500);
  }

  function saveAndUpdate(){
    const id = parseInt(document.getElementById("attendeeId").value,10);
    if(id<101||id>180) return showBanner("❌ Enter a number 101–180","error");
    if(!phoneMap[id]) return showBanner("❌ No phone assigned for ID "+id,"error");

    localStorage.setItem("attendeeId",id);
    updateConfig(id);
    showBanner("✅ Configuration updated!","success");
  }

  function resetId(){
    localStorage.removeItem("attendeeId");
    document.getElementById("attendeeId").value="";

    // Restore all ### placeholders
    document.body.innerHTML = document.body.innerHTML.replace(/\d+/g,"###");
    document.getElementById("channelPhone").innerText = "Not Assigned";
    showBanner("✅ Your ID has been reset!","success");
  }

  // Load saved ID on page load
  window.onload=function(){
    const savedId = localStorage.getItem("attendeeId");
    if(savedId){
      document.getElementById("attendeeId").value=savedId;
      updateConfig(parseInt(savedId,10));
    }
  }
</script>


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

   User Name:  WebexVoCX+admin_ID###@gmail.com

   Password:  Webex123!


2. Go to **Contact Center** from the left navigation panel → **AI Agents**  
??? Note "Show Me"
      ![](assets/CreateKB.gif)

3. Click on Build your AI Agent

4. In AI Agent Builder navigate to <strong>Knowledge</strong> from left hand side menu panel

5. Click <strong>Create Knowledge Base</strong>, provide Knowledge base name as ###_TravelAI_KB", then click <strong>Create</strong>.</p>

6. Click <strong>Add File</strong> or drag and drop file <strong>San Diego Travel AI Bot - Webex One Knowledge Base.pdf</strong> you downloaded from external drive on <strong>Step 1</strong>. Then click <strong>Process Files</strong>.</p>

7. Upload **San Diego Travel AI Bot - Webex One Knowledge Base.pdf** → **Process Files**  and Click the Back Arrow in the top Left after Processing is complete. 
??? Note "Show Me"
      ![](assets/UploadKB.gif)

### Creating the AI Agent

1. Go to **Dashboard → Create Agent**  

2. Choose Autonomous and Fill in the following details
    > Agent Name: **`###_TravelAI_Auto`**  
    > System ID: *auto generated*  
    > AI engine: **Webex AI Pro 1.0**  
    > Agents Goal: 
<div style="display: flex; align-items: center; gap: 10px;">
  <code id="copyPrompt" style="flex: 1;">
*You are a knowledgeable and personable travel assistant that helps customers manage, discover and enhance their vacations, delivering seamless support, solving problems quickly, finding the perfect restaurant and upselling relevant perks—all while showcasing the potential of AI in modern customer experience.*
  </code>
  <button onclick="copyToClipboard('copyPrompt')" style="font-weight: bold; padding: 6px 12px;">
    Copy
  </button>
</div>

??? Note "Show Me"
      ![](assets/CreateAIAgent.gif)

3. Once in the AI Agent Configuration and Enter the following Instructions:
    <p>
  <code id="copyPrompt1">
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
    </code>
</p>
<button onclick="copyToClipboard('copyPrompt1')">Copy</button>

??? Note "Show Me"
      ![](assets/InstructionAIAgent.gif)

4. In AI Agent Configuration → go to **Knowledge** Tab at the top.  
5. Go to **Knowledge** tab → select **`###_TravelAI_KB`** 
6. Click **Save Changes**
??? Note "Show Me"
      ![](assets/AttachKB.gif)

7. Go to **Actions** tab → turn ON **Agent handover**.   
8. Click **Save Changes → Publish** (enter version name like "1.0").  
??? Note "Show Me"
      ![](assets/AgentHandover.gif)

9. Click **Preview** → test by asking:  
    `"I'm looking for someplace to eat."`  
??? Note "Show Me"
      ![](assets/PreviewAgent.gif)


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

7. Dial the support number assigned to test.  

---

## Testing
1. Login into [Webex Agent Desktop](https://desktop.wxcc-us1.cisco.com/)  
   **`WebexVoCX+admin_ID###@gmail.com`**  
    `Password:  Webex123!` 
  
2. Select Team: **`###_Team`** → Submit → Allow Microphone Access.  
3. Change to Desktop as your Telephony Option 
4. Make your agent **Available**.  

??? Note "Show Me"
      ![](assets/AgentLogin.gif) 

5. Dial support number for   from your cell phone and ask about:  
      - Restaurants Nearby  
      - Things to do 

6. Request a **live agent** → call will be transferred.  

---
## Modifications and Adjusting


### Too Wordy
 If you’ve tried the Voice Bot, you may have noticed it can get a bit chatty—long, detailed instructions make it talk more than necessary. For a smooth, real-time conversation, Voice AI works best with short, snappy prompts. Think of it like talking to a friend: clear, direct, and to the point. Choosing the right words keeps the bot helpful, quick, and human-like, while still letting it show off its personality and capabilities.

### Modifying the AI Agent Instructions

1. Update the Instructions
Let’s change the previous instruction to something shorter and snappier. In the AI Agent Configuration, replace the existing instructions with one of these examples:

#### Short and Nice
You are a friendly travel assistant. Quickly summarize the user’s request and confirm understanding. Ask brief clarifying questions if needed, assuming the Marriott Marquis or nearby unless specified. After the main task, offer one quick, helpful suggestion like a nearby restaurant, coffee, or local attraction. Keep your tone warm, clear, and enthusiastic, using short natural sentences. Suggest upgrades or perks when relevant, gather key info (name, room, hotel), and always close with a thank you and a friendly offer for more help.

#### Sarcastic Version
You are a sarcastic, witty travel assistant. Keep your responses short—just a couple of sentences. Summarize the user’s request with a playful twist and confirm understanding, using humor to keep it light. Ask clarifying questions if needed, assuming the Marriott Marquis or nearby unless specified (“Another adventure, huh? Lucky you!”). After handling the request, offer one quick suggestion or perk with dry humor, like a local coffee spot or attraction (“Here’s a place you might survive a latte”). Suggest upgrades humorously, gather key info like name, room, and hotel, and close with a playful thank you and an offer for more “world-class assistance.”

#### Update the Welcome Message (Optional)
You can also tweak the Welcome Message to match the tone of your instructions. For example:

Friendly: "Hi there! I’m your travel assistant. How can I help make your trip easier today?"

Sarcastic: "Oh, another adventure, huh? Lucky you! What can I help you survive today?"

#### Test Your Changes
Once updated, call the Voice Bot and notice how the shorter, conversational prompts make interactions quicker, clearer, and more engaging. Try experimenting with tone—friendly, witty, or sarcastic—and see how it changes the user experience.


**🎉 Congratulations, you have officially completed the Autonomous Virtual Agent mission! 🎉**


<script>
function copyToClipboard(elementId) {
  const text = document.getElementById(elementId).innerText.trim();
  navigator.clipboard.writeText(text).then(() => {
    alert("Copied to clipboard!");
  }).catch(err => {
    alert("Failed to copy: " + err);
  });
}
</script>
