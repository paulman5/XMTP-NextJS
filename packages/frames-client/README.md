# Frames Client

This package is used by messaging apps to render Open Frames.

## Usage

```ts
const xmtpClient = await Client.create(wallet);
const framesClient = new FramesClient(xmtpClient);

const frameUrl = "https://www.myframe.xyz";
const framePostUrl = "https://www.myframe.xyz/api/post"; //fc:frame:post_url

// Read data from a frame
const frameMetadata = await framesClient.proxy.readMetadata(frameUrl);

// Get a proxied image URL, which you can use directly in an <image> tag
const imageUrl = framesClient.proxy.mediaUrl(
  frameMetadata.metaTags["fc:frame:image"],
);

// Handle a click to button 2 from a conversation with topic "/xmtp/0/123" and participant addresses "abc" and "xyz"
const textInputValue = "Your value";
const payload = await signFrameAction({
  frameUrl,
  inputText: textInputValue || undefined,
  buttonIndex: 2,
  conversationTopic: "/xmtp/0/123",
  participantAccountAddresses: ["abc", "xyz"],
  address: "0x...",
  state: frameMetadata?.frameInfo?.state, // Pulled from frame metadata
});

// If the button action type was `post`
const updatedFrameMetadata = await framesClient.proxy.post(
  framePostUrl,
  payload,
);
// If the button action type was `post_redirect`
const { redirectedTo } = await framesClient.proxy.postRedirect(
  framePostUrl,
  payload,
);
// If the button action type was `postTransaction`
const transactionInfo = await framesClient.proxy.postTransaction(
  framePostUrl,
  payload,
);
```
