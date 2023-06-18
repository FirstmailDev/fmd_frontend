<script>
  import firstmailLogo from "./assets/logo.svg";
  let email = "";
  let page = "home";
  let alert = { class: "alert hide", text: "", visible: false };
  let disabled = false;
  //load event does not work with vite hot reloading
  if (location.hash.startsWith("#")) {
    page = location.hash.substring(1);
  }
  const on_hash = async () => {
    page = location.hash.substring(1);
  };
  const on_submit = async (e) => {
    e.preventDefault(0);
    // console.log(`${email}`);
    disabled = true;
    alert = { class: "alert hide", text: "", visible: false };
    const res = await fetch("/api/user", {
      method: "POST",
      body: email,
    });
    // console.log(`${res.status}`);
    disabled = false;
    switch (res.status) {
      case 200:
        alert = {
          class: "alert alert-success mt-3",
          text: "A confirmation email has been sent.",
          visible: true,
        };
        break;
      default:
        alert = {
          class: "alert alert-danger mt-3",
          text: "Failure calling the Firstmail API.",
          visible: true,
        };
        break;
    }
  };
</script>

<svelte:window on:hashchange={on_hash} />

<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container">
    <a class="navbar-brand" href="https://firstmail.dev" rel="noreferrer">
      <img
        src={firstmailLogo}
        alt="Firstmail Logo"
        height="50"
        class="d-inline-block align-text-top"
      />
    </a>
    <button
      class="navbar-toggler"
      type="button"
      data-bs-toggle="collapse"
      data-bs-target="#navbarSupportedContent"
      aria-controls="navbarSupportedContent"
      aria-expanded="false"
      aria-label="Toggle navigation"
    >
      <span class="navbar-toggler-icon" />
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a
            class="nav-link"
            class:active={page == "home"}
            aria-current="page"
            href="#home">Home</a
          >
        </li>
        <li class="nav-item">
          <a class="nav-link" class:active={page == "faq"} href="#faq">FAQ</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" class:active={page == "privacy"} href="#privacy"
            >Privacy</a
          >
        </li>
        <li class="nav-item">
          <a class="nav-link" class:active={page == "terms"} href="#terms"
            >Terms</a
          >
        </li>
      </ul>
      <a
        class="nav-link"
        href="https://github.com/FirstmailDev"
        target="_blank"
        rel="noreferrer"
      >
        <i class="bi-github" />
      </a>
    </div>
  </div>
</nav>

<main class="container min-vh-100">
  <br />
  <br />
  {#if page == "home"}
    <form
      on:submit={on_submit}
      class="d-flex w-100 align-items-center justify-content-center mt-5 pt-5"
    >
      <!-- 
        hypen need scaping in a js regex bracket 
        an invalid regex disables input validation
      -->
      <input
        required
        type="text"
        pattern="^[\-_a-zA-Z0-9]+(\.[\-_a-zA-Z0-9]+)*@([\-_a-zA-Z0-9]+\.)+[a-zA-Z]+$"
        class="form-control me-2"
        style="max-width: 32em;"
        bind:value={email}
        placeholder="name@domain.com"
      />
      <button
        {disabled}
        type="submit"
        class="btn btn-success ms-2"
        title="Click to register or recover access">Register or Recover</button
      >
    </form>
    {#if alert.visible}
      <div class={alert.class} role="alert">
        {alert.text}
      </div>
    {/if}
  {/if}
  {#if page == "faq"}
    <h2>Frequently Asked Questions</h2>
    <br />
    <h3>What is Firstmail?</h3>
    <p>
      Firstmail is an open for development MTA (mail transport agent) with a web
      API that complies out of the box with all the security and anti-spam best
      practices like SPF, DMARC, and DKIM signatures. It is free to use for
      software development purposes.
    </p>
    <h3>How does it work?</h3>
    <p>
      When you register with Firstmail a confirmation email is sent to you with
      instructions on how to perform the next steps which include: (a) Setting
      your domain DNS records to comply with SPF, DMARC, and DKIM. (b) Sending
      email using our web API. (c) Optionally deleting your Firstmail
      registration.
    </p>
    <h3>Is it safe?</h3>
    <p>
      Firstmail uses security best practices and its API is only accesible over
      secure HTTP. Firstmail enforces email based authentication for each of its
      API operations. Firstmail will only send from the registered and verified
      custom domain email address once the custom DNS records have been setup.
    </p>
    <h3>Does it work with a gmail or yahoo email address?</h3>
    <p>
      Registering with a gmail or yahoo email address is not supported since you
      won't be able to setup the required DNS records for those domains. You
      need to register with a custom domain email address and you need to be
      able to modify the DNS records for that domain.
    </p>
    <h3>How do we get help?</h3>
    <p>
      We are happy to learn how to better serve you. Do not hesitate to share
      your concerns and ideas with us. Drop us a line at hello@firstmail.dev.
    </p>
    <h3>How do we report spam?</h3>
    <p>
      We take spam and unappropiated content very seriously. File a report by
      forwarding the email evidence to report@firstmail.dev.
    </p>
    <h3>Is it free?</h3>
    <p>
      Firstmail is committed to be a free service. If you find the service
      useful consider <a
        target="_blank"
        rel="noreferrer"
        href="https://patreon.com/Firstmail"
        >making a small donation or becoming a patron</a
      >.
    </p>
  {/if}
  {#if page == "privacy"}
    <h2>Privacy Policy</h2>
    <br />
    <ul>
      <li>Firstmail does not and will never use or store cookies.</li>
      <li>
        Firstmail does not and will never share your information with anyone.
      </li>
      <li>
        Firstmail does not and will never require or store any other information
        than the information provided by You to the Firstmail API.
      </li>
    </ul>
  {/if}
  {#if page == "terms"}
    <h2>Terms of Use</h2>
    <br />
    <p>
      Please read these Terms carefully before using this website. By using this
      website (the "Website"), the user ("You") has accepted these Terms of Use.
      If You do not accept these Terms of Use, do not use the Website. By using
      this Website, you represent that you are of legal age to form a binding
      contract with Firstmail.
    </p>
    <h3>You agree to the following rules:</h3>
    <p>
      You may not post or link to content that is threatening, abusive,
      defamatory, libelous, derogatory, violent, harassing, bigoted, hateful,
      profane, obscene, lewd, lascivious, pornographic or otherwise
      objectionable, that gives rise to civil or criminal liability, or
      otherwise violates any applicable law. You may not intentionally make
      false or misleading statements. You may not post material that infringes
      any copyright, trademark, patent, trade secret, right of privacy or right
      of publicity. You may not disclose confidential information. You may not
      disclose information that you do not lawfully possess. You may not post,
      link to, upload or transmit software or other materials that contain
      viruses, worms, time bombs, trojan horses, or other harmful or disruptive
      components. You may not restrict or inhibit any other user from using or
      enjoying this Website, for example, by cracking, spoofing, defacing, or
      impairing functionality.
    </p>
    <p>
      You are solely responsible for the content You provide. Firstmail has the
      right, but not the duty, to pre-screen, refuse, edit, move or remove any
      content that violates these Terms of Use or that is unlawful.
    </p>
    <p>
      LIMITATION ON LIABILITY. THIS WEBSITE AND ALL CONTENT, MATERIALS,
      INFORMATION, SOFTWARE, PRODUCTS AND SERVICES ARE PROVIDED ON AN "AS IS"
      AND "AS AVAILABLE" BASIS. YOUR USE OF THIS WEBSITE IS AT YOUR OWN RISK.
      GRABAKEY DISCLAIMS ALL WARRANTIES, EXPRESS OR IMPLIED, INCLUDING WITHOUT
      LIMITATION WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR
      PURPOSE, AND NONINFRINGEMENT. TO THE FULLEST EXTENT PERMITTED BY LAW, RED
      HAT IS NOT LIABLE FOR ANY DIRECT, INDIRECT, PUNITIVE, SPECIAL,
      INCIDENTAL,CONSEQUENTIAL, OR EXEMPLARY DAMAGES (INCLUDING, WITHOUT
      LIMITATION, LOSS OF BUSINESS, REVENUE, PROFITS, GOODWILL, USE, DATA,
      ELECTRONICALLY TRANSMITTED ORDERS, OR OTHER ECONOMIC ADVANTAGE) ARISING
      OUT OF OR IN CONNECTION WITH THE WEBSITE, EVEN IF GRABAKEY HAS PREVIOUSLY
      BEEN ADVISED OF, OR REASONABLY COULD HAVE FORESEEN, THE POSSIBILITY OF
      SUCH DAMAGES, HOWEVER THEY ARISE, WHETHER IN BREACH OF CONTRACT OR IN TORT
      (INCLUDING NEGLIGENCE), INCLUDING WITHOUT LIMITATION DAMAGES DUE TO (a)
      THE USE OF OR THE INABILITY TO USE THE WEBSITE; (b) THE COST OF
      PROCUREMENT OF SUBSTITUTE GOODS AND SERVICES RESULTING FROM ANY GOODS,
      DATA, INFORMATION OR SERVICES PURCHASED OR OBTAINED OR MESSAGES RECEIVED
      OR TRANSACTIONS ENTERED INTO, THROUGH OR FROM THE WEBSITE; ( c) STATEMENTS
      OR CONDUCT OF ANY THIRD PARTY ON THE WEBSITE, INCLUDING WITHOUT LIMITATION
      UNAUTHORIZED ACCESS TO OR ALTERATION OF TRANSMISSIONS OR DATA, MALICIOUS
      OR CRIMINAL BEHAVIOR, OR FALSE OR FRAUDULENT TRANSACTIONS, OR (d) CONTENT
      OR INFORMATION YOU MAY DOWNLOAD, USE, MODIFY OR DISTRIBUTE. GRABAKEY MAKES
      NO WARRANTY THAT, (i) THE SERVICE WILL BE UNINTERRUPTED, TIMELY, SECURE,
      OR ERROR-FREE, (ii) THE RESULTS THAT MAY BE OBTAINED FROM THE USE OF THE
      SERVICE WILL BE ACCURATE OR RELIABLE, (iii) THE QUALITY OF ANY PRODUCTS,
      SERVICES, CONTENT, INFORMATION, OR OTHER MATERIALS OBTAINED BY YOU THROUGH
      THE SERVICE WILL MEET YOUR EXPECTATIONS, (iv) ANY ERRORS IN THE SOFTWARE
      WILL BE CORRECTED, OR THAT THIS WEBSITE, ITS CONTENT, AND THE SERVER ON
      WHICH THE WEBSITE AND CONTENT ARE AVAILABLE ARE FREE OF VIRUSES OR OTHER
      HARMFUL COMPONENTS. ANY MATERIAL (INCLUDING CONTENT) DOWNLOADED OR
      OBTAINED THROUGH THE USE OF THIS WEBSITE IS DONE AT YOUR OWN RISK AND YOU
      WILL BE SOLELY RESPONSIBLE FOR ANY DAMAGE TO YOUR COMPUTER SYSTEM OR LOSS
      OF DATA THAT RESULTS FROM THE DOWNLOAD OF ANY MATERIAL. INFORMATION
      CREATED BY THIRD PARTIES THAT YOU MAY ACCESS ON THIS WEBSITE OR THROUGH
      LINKS IS NOT APPROVED, ADOPTED OR ENDORSED BY GRABAKEY AND REMAINS THE
      RESPONSIBILITY OF THE THIRD PARTY. TO THE EXTENT ANY JURISDICTION DOES NOT
      ALLOW THE EXCLUSION OR LIMITATION OF DIRECT, INCIDENTAL OR CONSEQUENTIAL
      DAMAGES, PORTIONS OF THE ABOVE LIMITATION OR EXCLUSION MAY NOT APPLY.
    </p>
  {/if}
</main>

<footer class="text-center text-lg-start bg-light text-muted mt-5">
  <div class="text-center p-4" style="background-color: rgba(0, 0, 0, 0.05);">
    © 2023 firstmail.dev
  </div>
</footer>

<style>
</style>
