<h1>Active Directory Lab 2: Active Directory Installation &amp; Domain Join</h1>

<h2>Project Summary</h2>
<p>
  In this lab, I install Active Directory Domain Services (AD DS) on the Domain Controller (DC-1) and promote it to a Domain Controller for a new forest (<code>mydomain.com</code>).
  Additionally, I create a Domain Admin user and join the client machine (Client-1) to the domain.
  This setup simulates a real-world Active Directory environment.
</p>

<ul>
  <li><strong>Key Components:</strong>
    <ul>
      <li>DC-1: Windows Server 2022 Datacenter</li>
      <li>Client-1: Windows 10 Pro</li>
      <li>AD DS installation and promotion</li>
      <li>Domain Admin user creation</li>
      <li>Domain join of Client-1</li>
    </ul>
  </li>
  <li><strong>Goal:</strong> Install and configure AD DS on DC-1, then join Client-1 to the domain.</li>
</ul>

<hr />

<h2>Steps &amp; Screenshots</h2>
<ol>
  <li>
    <strong>Remote into DC-1 and Confirm Environment</strong><br />
    Copy the public IP of DC-1 from Azure and use Remote Desktop Connection to log in with your <code>labuser</code> credentials.
    <p align="center">
      <img src="https://i.imgur.com/FK7F2Yi.png" alt="Remote Desktop Connection to DC-1" width="600" />
      <br /><em>Figure 2.1 – Connecting to DC-1 via Remote Desktop.</em>
    </p>
    Confirm that DC-1 is running Windows Server by checking <strong>Settings &gt; System &gt; About</strong>.
    <p align="center">
      <img src="https://i.imgur.com/rVRqVA3.png" alt="DC-1 system information" width="600" />
      <br /><em>Figure 2.2 – System information showing Windows Server.</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Install Active Directory Domain Services (AD DS)</strong><br />
    Open <strong>Server Manager</strong> on DC-1 and click on <strong>Add Roles and Features</strong>. Select <strong>Active Directory Domain Services</strong> from the roles list and proceed with the installation.
    <p align="center">
      <img src="https://i.imgur.com/MVhTNoQ.png" alt="AD DS role selection in Server Manager" width="600" />
      <br /><em>Figure 2.3 – Selecting AD DS in the Add Roles and Features Wizard.</em>
    </p>
    Complete the wizard, opting for a system restart after installation.
    <p align="center">
      <img src="https://i.imgur.com/AtcdKIX.png" alt="AD DS installation progress" width="600" />
      <br /><em>Figure 2.4 – AD DS installation in progress.</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Promote DC-1 as the Domain Controller</strong><br />
    After the restart, open Server Manager and click the flag icon to select <strong>Promote this server to a domain controller</strong>. In the wizard, choose <strong>Add a new forest</strong> and enter <code>mydomain.com</code> as the root domain name. Provide the Directory Services Restore Mode (DSRM) password (<code>Password1</code>) and proceed with the default settings.
    <p align="center">
      <img src="https://i.imgur.com/5RuUmXS.png" alt="Promoting DC-1 to Domain Controller" width="600" />
      <img src="https://i.imgur.com/GvA5w3w.png" alt="Promoting DC-1 to Domain Controller" width="600" />
      <br /><em>Figure 2.5 – Configuring a new forest and promoting DC-1.</em>
    </p>
    The system will restart upon completion; log back in and note that your login now shows <code>mydomain.com\labuser</code>.
  </li>
  <br />

  <li>
    <strong>Create a Domain Admin User</strong><br />
    Open <strong>Active Directory Users and Computers</strong> (ADUC) from Administrative Tools. Create two Organizational Units (OUs) under <code>mydomain.com</code>—for example, <code>_EMPLOYEES</code> and <code>_ADMINS</code>. Next, create a new user named <strong>Jane Doe</strong> with the logon name <code>jane_admin@mydomain.com</code> and password <code>Cyberlab123!</code>. Then, add Jane Doe to the <strong>Domain Admins</strong> group.
    <p align="center">
      <img src="https://i.imgur.com/2ZZG8nG.png" alt="Creating Domain Admin user in ADUC" width="600" />
      <img src="https://i.imgur.com/4dVOsI9.png" alt="Creating Domain Admin user in ADUC" width="600" />
      <br /><em>Figure 2.6 – Creating Jane Doe as a Domain Admin in ADUC.</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Join Client-1 to the Domain</strong><br />
    On Client-1, log in with the local <code>labuser</code> credentials. Navigate to <strong>Settings &gt; System &gt; About &gt; Advanced system settings</strong> and click <strong>Change</strong> under the Computer Name tab. Select <strong>Domain</strong> and enter <code>mydomain.com</code>, then provide the Domain Admin credentials (<code>mydomain.com\jane_admin</code>) when prompted.
    <p align="center">
      <img src="https://i.imgur.com/0B3AMsA.png" alt="Joining Client-1 to the domain" width="600" />
      <br /><em>Figure 2.7 – Configuring Client-1 to join the domain.</em>
    </p>
    Client-1 will restart to complete the process.
  </li>
  <br />

  <li>
    <strong>Verify Domain Join in Active Directory</strong><br />
    Return to DC-1 and open <strong>Active Directory Users and Computers</strong>. Expand <code>mydomain.com</code> and click on the <strong>Computers</strong> container to verify that Client-1 appears. Optionally, create an OU called <code>_CLIENTS</code> and move Client-1 into it.
    <p align="center">
      <img src="https://i.imgur.com/nAmKt7w.png" alt="Verifying Client-1 in ADUC" width="600" />
      <br /><em>Figure 2.8 – Client-1 is visible in the Computers container in ADUC.</em>
    </p>
  </li>
</ol>

<hr />

<h2>Conclusion</h2>
<p>
  In this lab, I successfully installed AD DS on DC-1, promoted it as the Domain Controller for a new forest (<code>mydomain.com</code>), and created a Domain Admin user.
  I also joined Client-1 to the domain, as confirmed via Active Directory Users and Computers.
  This setup forms the foundation for advanced Active Directory management and administration tasks in subsequent labs.
</p>
