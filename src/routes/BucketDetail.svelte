<script>
  export let params = {};
  import CountDown from "../components/CountDown.svelte";
  import ImageCard from "../components/ImageCard.svelte";
  import NavBar from "../components/NavBar.svelte";

  const userID = params.userid;
  const bucketID = params.bucketid;
  //console.log(userID, bucketID);
  let data = {};
  fillData();
  function fillData() {
    // servercall
    const freshdatafromserver = {
      bucketID: "bucketID",
      userID: "userID",
      isAdmin: false,
      bucketName: "Bucket #1",
      userName: "Danny Boi",
      targetDate: "Jan 17, 2022",
      winnerImage: "imag1", // imageID
      imageCardDetails: [
        {
          imageID: "imag1",
          imgURL: "https://picsum.photos/400",
          votes: {
            upvotes: 50,
            downvotes: 30,
          },
          reactions: {
            confetti: 0,
            wow: 0,
            heart: 0,
            dislike: 0,
          },
          voted: "notvoted",
          reacted: "notreacted",
        },
        {
          imageID: "imag2",
          imgURL: "https://picsum.photos/400",
          votes: {
            upvotes: 20,
            downvotes: 30,
          },
          reactions: {
            confetti: 0,
            wow: 0,
            heart: 0,
            dislike: 0,
          },
          voted: "notvoted",
          reacted: "notreacted",
        },
        {
          imageID: "imag3",
          imgURL: "https://picsum.photos/400",
          votes: {
            upvotes: 20,
            downvotes: 30,
          },
          reactions: {
            confetti: 0,
            wow: 0,
            heart: 0,
            dislike: 0,
          },
          voted: "notvoted",
          reacted: "notreacted",
        },
      ],
    };
    data = freshdatafromserver;
    console.log(data.imageCardDetails);
  }
  function upvote(imageID) {
    //console.log(imageID);
    let item = data.imageCardDetails.find((x) => x.imageID === imageID);

    if (item.voted === "notvoted") {
      item.votes.upvotes += 1;
      item.voted = "upvoted";
      data.imageCardDetails = [...data.imageCardDetails];
      // update on server
    } else if (item.voted === "downvoted") {
      item.votes.upvotes += 1;
      item.votes.downvotes -= 1;
      item.voted = "upvoted";
      data.imageCardDetails = [...data.imageCardDetails];
      // update on server
    } else {
      item.votes.upvotes -= 1;
      item.voted = "notvoted";
      data.imageCardDetails = [...data.imageCardDetails];
      //alert("Already voted, stop pressing random buttons you moron");
    }

    console.log(data.imageCardDetails);
  }
  function downvote(imageID) {
    //console.log(imageID);
    let item = data.imageCardDetails.find((x) => x.imageID === imageID);
    if (item.voted === "notvoted") {
      item.votes.downvotes += 1;
      item.voted = "downvoted";
      data.imageCardDetails = [...data.imageCardDetails];
      // update on server
    } else if (item.voted === "upvoted") {
      item.votes.upvotes -= 1;
      item.votes.downvotes += 1;
      item.voted = "downvoted";
      data.imageCardDetails = [...data.imageCardDetails];
      // update on server
    } else {
      item.votes.downvotes -= 1;
      item.voted = "notvoted";
      data.imageCardDetails = [...data.imageCardDetails];
      //alert("Already voted, stop pressing random buttons you moron");
    }
  }
  function updateSelectedReaction(imageID, reaction) {
    let item = data.imageCardDetails.find((x) => x.imageID === imageID);
    if (item.reacted === "notreacted") {
      item.reactions[reaction]++;
      item.reacted = reaction;
      //send to server
    } else {
      item.reactions[item.reacted]--;
      item.reactions[reaction]++;
      item.reacted = reaction;
      // send to server
    }
    console.log(data.imageCardDetails);
  }
  function clearSelectedReaction(imageID) {
    let item = data.imageCardDetails.find((x) => x.imageID === imageID);
    item.reactions[item.reacted]--;
    item.reacted = "notreacted";
    // send to server
    console.log(data.imageCardDetails);
  }
</script>

<section class=" h-screen flex  flex-col  p-5 pt-2.5 lg:p-10 lg:pt-5 ">
  <NavBar />
  <div class="mx-auto">
    <div class="flex  justify-between lg:items-center lg:flex-row flex-col ">
      <div>
        <div class="text-4xl mt-10 font-bold">{data.bucketName}</div>
        <div class="flex items-center gap-5 mt-3 ">
          <div class="avatar">
            <div class=" w-10 h-10 mask mask-squircle">
              <img
                src="http://daisyui.com/tailwind-css-component-profile-1@94w.png"
              />
            </div>
          </div>
          <div class="opacity-70 newfont text-xl">by {data.userName}</div>
        </div>
      </div>
      <div class="mt-5 flex gap-7">
        <div class="flex gap-3 ">
          <CountDown targetDate={new Date(data.targetDate)} />
        </div>
      </div>
    </div>
    <div class="grid mt-10 lg:grid-cols-3 gap-10">
      {#each data.imageCardDetails as ImageCardDetail}
        <ImageCard
          isWinner={data.winnerImage === ImageCardDetail.imageID}
          imgURL={ImageCardDetail.imgURL}
          reaction={ImageCardDetail.reactions}
          votes={ImageCardDetail.votes}
          imageID={ImageCardDetail.imageID}
          isAdmin={data.isAdmin}
          voted={ImageCardDetail.voted}
          {upvote}
          {downvote}
          {updateSelectedReaction}
          {clearSelectedReaction}
        />
      {/each}
    </div>
  </div>
</section>

<style>
  .newfont {
    font-family: "Harmattan", sans-serif;
  }
</style>
