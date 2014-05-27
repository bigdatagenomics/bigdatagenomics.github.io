---
layout: page
title: "Corporate Contributor License Agreement"
date: 2014-05-14 09:03
comments: false
sharing: true
footer: true
---

For more details about this agreement, [visit the contributor license agreement page](/cla/).
You can digitally sign this agreement at the bottom of this page.

```

                      The Big Data Genomics Project
  Software Grant and Corporate Contributor License Agreement ("Agreement")

   Thank you for your interest in The Big Data Genomics Project (the
   "BDG Project"). In order to clarify the intellectual property license
   granted with Contributions from any person or entity, the BDG Project
   must have a Contributor License Agreement (CLA) on file that has been
   signed by each Contributor, indicating agreement to the license terms
   below. This license is for your protection as a Contributor as well
   as the protection of the BDG Project and its users; it does not change
   your rights to use your own Contributions for any other purpose.

   This version of the Agreement allows an entity (the "Corporation") to
   submit Contributions to the BDG Project, to authorize Contributions 
   submitted by its designated employees to the BDG Project, and to grant 
   copyright and patent licenses thereto.

   You accept and agree to the following terms and conditions for Your
   present and future Contributions submitted to the BDG Project. In
   return, the BDG Project shall not use Your Contributions in a way that
   is contrary to the public benefit or inconsistent with its nonprofit
   status and bylaws in effect at the time of the Contribution. Except
   for the license granted herein to the BDG Project and recipients of
   software distributed by the BDG Project, You reserve all right, title,
   and interest in and to Your Contributions.

   1. Definitions.

      "You" (or "Your") shall mean the copyright owner or legal entity
      authorized by the copyright owner that is making this Agreement
      with the BDG Project. For legal entities, the entity making a
      Contribution and all other entities that control, are controlled by,
      or are under common control with that entity are considered to be a
      single Contributor. For the purposes of this definition, "control"
      means (i) the power, direct or indirect, to cause the direction or
      management of such entity, whether by contract or otherwise, or
      (ii) ownership of fifty percent (50%) or more of the outstanding
      shares, or (iii) beneficial ownership of such entity.

      "Contribution" shall mean the code, documentation or other original
      works of authorship expressly identified in Schedule B, as well as
      any original work of authorship, including
      any modifications or additions to an existing work, that is intentionally
      submitted by You to the BDG Project for inclusion in, or
      documentation of, any of the products owned or managed by the
      BDG Project (the "Work"). For the purposes of this definition,
      "submitted" means any form of electronic, verbal, or written
      communication sent to the BDG Project or its representatives,
      including but not limited to communication on electronic mailing
      lists, source code control systems, and issue tracking systems
      that are managed by, or on behalf of, the BDG Project for the
      purpose of discussing and improving the Work, but excluding
      communication that is conspicuously marked or otherwise designated
      in writing by You as "Not a Contribution."

   2. Grant of Copyright License. Subject to the terms and conditions
      of this Agreement, You hereby grant to the BDG Project and to
      recipients of software distributed by the BDG Project a perpetual,
      worldwide, non-exclusive, no-charge, royalty-free, irrevocable
      copyright license to reproduce, prepare derivative works of,
      publicly display, publicly perform, sublicense, and distribute
      Your Contributions and such derivative works.

   3. Grant of Patent License. Subject to the terms and conditions of
      this Agreement, You hereby grant to the BDG Project and to recipients
      of software distributed by the BDG Project a perpetual, worldwide,
      non-exclusive, no-charge, royalty-free, irrevocable (except as
      stated in this section) patent license to make, have made, use,
      offer to sell, sell, import, and otherwise transfer the Work,
      where such license applies only to those patent claims licensable
      by You that are necessarily infringed by Your Contribution(s)
      alone or by combination of Your Contribution(s) with the Work to
      which such Contribution(s) were submitted. If any entity institutes
      patent litigation against You or any other entity (including a
      cross-claim or counterclaim in a lawsuit) alleging that your
      Contribution, or the Work to which you have contributed, constitutes
      direct or contributory patent infringement, then any patent licenses
      granted to that entity under this Agreement for that Contribution or
      Work shall terminate as of the date such litigation is filed.

   4. You represent that You are legally entitled to grant the above
      license. You represent further that each employee of the
      Corporation designated on Schedule A below (or in a subsequent
      written modification to that Schedule) is authorized to submit
      Contributions on behalf of the Corporation.

   5. You represent that each of Your Contributions is Your original
      creation (see section 7 for submissions on behalf of others).

   6. You are not expected to provide support for Your Contributions,
      except to the extent You desire to provide support. You may provide
      support for free, for a fee, or not at all. Unless required by
      applicable law or agreed to in writing, You provide Your
      Contributions on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS
      OF ANY KIND, either express or implied, including, without
      limitation, any warranties or conditions of TITLE, NON-INFRINGEMENT,
      MERCHANTABILITY, or FITNESS FOR A PARTICULAR PURPOSE.

   7. Should You wish to submit work that is not Your original creation,
      You may submit it to the BDG Project separately from any
      Contribution, identifying the complete details of its source and
      of any license or other restriction (including, but not limited
      to, related patents, trademarks, and license agreements) of which
      you are personally aware, and conspicuously marking the work as
      "Submitted on behalf of a third-party: [named here]".

   8. It is your responsibility to notify the BDG Project when any change
      is required to the list of designated employees authorized to submit
      Contributions on behalf of the Corporation, or to the Corporation's
      Point of Contact with the BDG Project.
```

<link href="/cla/1590786475-formview_embedded_ltr.css" type="text/css" rel="stylesheet">
<style type="text/css">

</style>


<style type="text/css">
      
    </style>
<script type="text/javascript">
      /**
 * @license
 *! H5F
 * https://github.com/ryanseddon/H5F/
 * Copyright (c) Ryan Seddon | Licensed MIT
 */

(function(e,t){"function"==typeof define&&define.amd?define(t):e.H5F=t()})(this,function(){var e,t,a,i,n,r,s,l,u,o,c,d,v,f,p,m,h,g,b,y,w,C,N,A,E,$,k=document,x=k.createElement("input"),q=/^[a-zA-Z0-9.!#$%&'*+-\/=?\^_`{|}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$/,M=/[a-z][\-\.+a-z]*:\/\//i,L=/^(input|select|textarea)$/i;return r=function(e,t){var a=!e.nodeType||!1,i={validClass:"valid",invalidClass:"error",requiredClass:"required",placeholderClass:"placeholder"};if("object"==typeof t)for(var r in i)t[r]===void 0&&(t[r]=i[r]);if(n=t||i,a)for(var l=0,u=e.length;u>l;l++)s(e[l]);else s(e)},s=function(a){var i,n=a.elements,r=n.length,s=!!a.attributes.novalidate;if(b(a,"invalid",u,!0),b(a,"blur",u,!0),b(a,"input",u,!0),b(a,"keyup",u,!0),b(a,"focus",u,!0),b(a,"change",u,!0),b(a,"click",o,!0),b(a,"submit",function(i){e=!0,t||s||a.checkValidity()||w(i)},!1),!v())for(a.checkValidity=function(){return c(a)};r--;)i=!!n[r].attributes.required,"fieldset"!==n[r].nodeName.toLowerCase()&&l(n[r])},l=function(e){var t=e,a=g(t),n={type:t.getAttribute("type"),pattern:t.getAttribute("pattern"),placeholder:t.getAttribute("placeholder")},r=/^(email|url)$/i,s=/^(input|keyup)$/i,l=r.test(n.type)?n.type:n.pattern?n.pattern:!1,u=f(t,l),o=m(t,"step"),v=m(t,"min"),h=m(t,"max"),b=!(""===t.validationMessage||void 0===t.validationMessage);t.checkValidity=function(){return c.call(this,t)},t.setCustomValidity=function(e){d.call(t,e)},t.validity={valueMissing:a,patternMismatch:u,rangeUnderflow:v,rangeOverflow:h,stepMismatch:o,customError:b,valid:!(a||u||o||v||h||b)},n.placeholder&&!s.test(i)&&p(t)},u=function(e){var t=C(e)||e,a=/^(input|keyup|focusin|focus|change)$/i,r=/^(submit|image|button|reset)$/i,s=/^(checkbox|radio)$/i,o=!0;!L.test(t.nodeName)||r.test(t.type)||r.test(t.nodeName)||(i=e.type,v()||l(t),t.validity.valid&&(""!==t.value||s.test(t.type))||t.value!==t.getAttribute("placeholder")&&t.validity.valid?(A(t,[n.invalidClass,n.requiredClass]),N(t,n.validClass)):a.test(i)?t.validity.valueMissing&&A(t,[n.requiredClass,n.invalidClass,n.validClass]):t.validity.valueMissing?(A(t,[n.invalidClass,n.validClass]),N(t,n.requiredClass)):t.validity.valid||(A(t,[n.validClass,n.requiredClass]),N(t,n.invalidClass)),"input"===i&&o&&(y(t.form,"keyup",u,!0),o=!1))},c=function(t){var a,i,n,r,s=!1;if("form"===t.nodeName.toLowerCase()){a=t.elements;for(var l=0,o=a.length;o>l;l++)i=a[l],n=!!i.attributes.required,r=!!i.attributes.pattern,"fieldset"!==i.nodeName.toLowerCase()&&(n||r&&n)&&(u(i),i.validity.valid||s||(e&&i.focus(),s=!0));return!s}return u(t),t.validity.valid},d=function(e){var t=this;t.validationMessage=e},o=function(e){var a=C(e);a.attributes.formnovalidate&&"submit"===a.type&&(t=!0)},v=function(){return E(x,"validity")&&E(x,"checkValidity")},f=function(e,t){if("email"===t)return!q.test(e.value);if("url"===t)return!M.test(e.value);if(t){var i=e.getAttribute("placeholder"),n=e.value;return a=RegExp("^(?:"+t+")$"),n===i?!1:""===n?!1:!a.test(e.value)}return!1},p=function(e){var t={placeholder:e.getAttribute("placeholder")},a=/^(focus|focusin|submit)$/i,r=/^(input|textarea)$/i,s=/^password$/i,l=!!("placeholder"in x);l||!r.test(e.nodeName)||s.test(e.type)||(""!==e.value||a.test(i)?e.value===t.placeholder&&a.test(i)&&(e.value="",A(e,n.placeholderClass)):(e.value=t.placeholder,b(e.form,"submit",function(){i="submit",p(e)},!0),N(e,n.placeholderClass)))},m=function(e,t){var a=parseInt(e.getAttribute("min"),10)||0,i=parseInt(e.getAttribute("max"),10)||!1,n=parseInt(e.getAttribute("step"),10)||1,r=parseInt(e.value,10),s=(r-a)%n;return g(e)||isNaN(r)?"number"===e.getAttribute("type")?!0:!1:"step"===t?e.getAttribute("step")?0!==s:!1:"min"===t?e.getAttribute("min")?a>r:!1:"max"===t?e.getAttribute("max")?r>i:!1:void 0},h=function(e){var t=!!e.attributes.required;return t?g(e):!1},g=function(e){var t=e.getAttribute("placeholder"),a=/^(checkbox|radio)$/i,i=!!e.attributes.required;return!(!i||""!==e.value&&e.value!==t&&(!a.test(e.type)||$(e)))},b=function(e,t,a,i){E(window,"addEventListener")?e.addEventListener(t,a,i):E(window,"attachEvent")&&window.event!==void 0&&("blur"===t?t="focusout":"focus"===t&&(t="focusin"),e.attachEvent("on"+t,a))},y=function(e,t,a,i){E(window,"removeEventListener")?e.removeEventListener(t,a,i):E(window,"detachEvent")&&window.event!==void 0&&e.detachEvent("on"+t,a)},w=function(e){e=e||window.event,e.stopPropagation&&e.preventDefault?(e.stopPropagation(),e.preventDefault()):(e.cancelBubble=!0,e.returnValue=!1)},C=function(e){return e=e||window.event,e.target||e.srcElement},N=function(e,t){var a;e.className?(a=RegExp("(^|\\s)"+t+"(\\s|$)"),a.test(e.className)||(e.className+=" "+t)):e.className=t},A=function(e,t){var a,i,n="object"==typeof t?t.length:1,r=n;if(e.className)if(e.className===t)e.className="";else for(;n--;)a=RegExp("(^|\\s)"+(r>1?t[n]:t)+"(\\s|$)"),i=e.className.match(a),i&&3===i.length&&(e.className=e.className.replace(a,i[1]&&i[2]?" ":""))},E=function(e,t){var a=typeof e[t],i=RegExp("^function|object$","i");return!!(i.test(a)&&e[t]||"unknown"===a)},$=function(e){for(var t=document.getElementsByName(e.name),a=0;t.length>a;a++)if(t[a].checked)return!0;return!1},{setup:r}});

    </script>
<style id="clearly_highlighting_css" type="text/css">/* selection */ html.clearly_highlighting_enabled ::-moz-selection { background: rgba(246, 238, 150, 0.99); } html.clearly_highlighting_enabled ::selection { background: rgba(246, 238, 150, 0.99); } /* cursor */ html.clearly_highlighting_enabled {    /* cursor and hot-spot position -- requires a default cursor, after the URL one */    cursor: url("chrome-extension://pioclpoplcdbaefihamjohnefbikjilc/clearly/images/highlight--cursor.png") 14 16, text; } /* highlight tag */ em.clearly_highlight_element {    font-style: inherit !important; font-weight: inherit !important;    background-image: url("chrome-extension://pioclpoplcdbaefihamjohnefbikjilc/clearly/images/highlight--yellow.png");    background-repeat: repeat-x; background-position: top left; background-size: 100% 100%; } /* the delete-buttons are positioned relative to this */ em.clearly_highlight_element.clearly_highlight_first { position: relative; } /* delete buttons */ em.clearly_highlight_element a.clearly_highlight_delete_element {    display: none; cursor: pointer;    padding: 0; margin: 0; line-height: 0;    position: absolute; width: 34px; height: 34px; left: -17px; top: -17px;    background-image: url("chrome-extension://pioclpoplcdbaefihamjohnefbikjilc/clearly/images/highlight--delete-sprite.png"); background-repeat: no-repeat; background-position: 0px 0px; } em.clearly_highlight_element a.clearly_highlight_delete_element:hover { background-position: -34px 0px; } /* retina */ @media (min--moz-device-pixel-ratio: 2), (-webkit-min-device-pixel-ratio: 2), (min-device-pixel-ratio: 2) {    em.clearly_highlight_element { background-image: url("chrome-extension://pioclpoplcdbaefihamjohnefbikjilc/clearly/images/highlight--yellow@2x.png"); }    em.clearly_highlight_element a.clearly_highlight_delete_element { background-image: url("chrome-extension://pioclpoplcdbaefihamjohnefbikjilc/clearly/images/highlight--delete-sprite@2x.png"); background-size: 68px 34px; } } </style><link type="text/css" rel="stylesheet" href="chrome-extension://cpngackimfmofbokmjmljamhdncknpmg/style.css"><script type="text/javascript" charset="utf-8" src="chrome-extension://cpngackimfmofbokmjmljamhdncknpmg/js/page_context.js"></script><style>[touch-action="none"]{ -ms-touch-action: none; touch-action: none; }[touch-action="pan-x"]{ -ms-touch-action: pan-x; touch-action: pan-x; }[touch-action="pan-y"]{ -ms-touch-action: pan-y; touch-action: pan-y; }[touch-action="scroll"],[touch-action="pan-x pan-y"],[touch-action="pan-y pan-x"]{ -ms-touch-action: pan-x pan-y; touch-action: pan-x pan-y; }</style>

<div class="ss-form-container"><div class="ss-header-image"></div>
<div class="ss-top-of-page"><div class="ss-form-heading"><h1 class="ss-form-title" dir="ltr">Sign Electronically</h1>


<hr class="ss-email-break" style="display:none;">
<div class="ss-required-asterisk" aria-hidden="true">* Required</div></div></div>
<div class="ss-form"><form action="https://docs.google.com/a/massie.us/forms/d/1MWdnKZHfSiIf8OW0TvCLZh_yStLkxulozjjZEPUTOmU/formResponse?embedded=true" method="POST" id="ss-form" target="_self" onsubmit=""><ol role="list" class="ss-question-list" style="padding-left: 0">
<div class="ss-form-question errorbox-good" role="listitem">
<div dir="ltr" class="ss-item ss-item-required ss-text"><div class="ss-form-entry">
<label class="ss-q-item-label" for="entry_2137761539"><div class="ss-q-title">Corporation Name
<label for="itemView.getDomIdToLabel()" aria-label="(Required field)"></label>
<span class="ss-required-asterisk">*</span></div>
<div class="ss-q-help ss-secondary-text" dir="ltr"></div></label>
<input type="text" name="entry.2137761539" value="" class="ss-q-short required" id="entry_2137761539" dir="auto" aria-label="Corporation Name  " aria-required="true" required="" title="">
<div class="error-message"></div>
<div class="required-message">This is a required question</div>
</div></div></div> <div class="ss-form-question errorbox-good" role="listitem">
<div dir="ltr" class="ss-item ss-item-required ss-paragraph-text"><div class="ss-form-entry">
<label class="ss-q-item-label" for="entry_1671947736"><div class="ss-q-title">Corporation Address
<label for="itemView.getDomIdToLabel()" aria-label="(Required field)"></label>
<span class="ss-required-asterisk">*</span></div>
<div class="ss-q-help ss-secondary-text" dir="ltr"></div></label>
<textarea name="entry.1671947736" rows="8" cols="0" class="ss-q-long" id="entry_1671947736" dir="auto" aria-label="Corporation Address  " aria-required="true" required=""></textarea>
<div class="error-message"></div>
<div class="required-message">This is a required question</div>
</div></div></div> <div class="ss-form-question errorbox-good" role="listitem">
<div dir="ltr" class="ss-item ss-item-required ss-paragraph-text"><div class="ss-form-entry">
<label class="ss-q-item-label" for="entry_1805226645"><div class="ss-q-title">Schedule A
<label for="itemView.getDomIdToLabel()" aria-label="(Required field)"></label>
<span class="ss-required-asterisk">*</span></div>
<div class="ss-q-help ss-secondary-text" dir="ltr">List of github usernames allowed to contribute code (separated by spaces). This authorization is not tied to a particular contribution and applies to all projects in the Big Data Genomics github organization (<a href="https://www.google.com/url?q=https%3A%2F%2Fgithub.com%2Fbigdatagenomics%2F&sa=D&sntz=1&usg=AFQjCNHfdEbl8QQhZiDBy08BQ5l2LpCVHg">https://github.com/bigdatagenomics/</a>). Once your CCLA is created, you can modify the authorized users by sending an email to <a href="mailto:cla@bdgenomics.org">cla@bdgenomics.org</a>.</div></label>
<textarea name="entry.1805226645" rows="8" cols="0" class="ss-q-long" id="entry_1805226645" dir="auto" aria-label="Schedule A List of github usernames allowed to contribute code (separated by spaces). This authorization is not tied to a particular contribution and applies to all projects in the Big Data Genomics github organization (https://github.com/bigdatagenomics/). Once your CCLA is created, you can modify the authorized users by sending an email to cla@bdgenomics.org. " aria-required="true" required=""></textarea>
<div class="error-message"></div>
<div class="required-message">This is a required question</div>
</div></div></div> <div class="ss-form-question errorbox-good" role="listitem">
<div dir="ltr" class="ss-item  ss-paragraph-text"><div class="ss-form-entry">
<label class="ss-q-item-label" for="entry_305011223"><div class="ss-q-title">Schedule B
</div>
<div class="ss-q-help ss-secondary-text" dir="ltr">Identification of optional concurrent software grant.  Would be left blank or omitted if there is no concurrent software grant.</div></label>
<textarea name="entry.305011223" rows="8" cols="0" class="ss-q-long" id="entry_305011223" dir="auto" aria-label="Schedule B Identification of optional concurrent software grant.  Would be left blank or omitted if there is no concurrent software grant. "></textarea>
<div class="error-message"></div>
<div class="required-message">This is a required question</div>
</div></div></div> <div class="errorbox-good" role="listitem">
<div dir="ltr" class="ss-item  ss-section-header"><div class="ss-form-entry">
<h2 class="ss-section-title">Point of Contact</h2>
<div class="ss-section-description ss-no-ignore-whitespace">Information about the point of contact at the corporation with the authority to grant employees the right to work on Big Data Genomics software projects</div>
</div></div></div> <div class="ss-form-question errorbox-good" role="listitem">
<div dir="ltr" class="ss-item ss-item-required ss-text"><div class="ss-form-entry">
<label class="ss-q-item-label" for="entry_896919823"><div class="ss-q-title">Full Name
<label for="itemView.getDomIdToLabel()" aria-label="(Required field)"></label>
<span class="ss-required-asterisk">*</span></div>
<div class="ss-q-help ss-secondary-text" dir="ltr"></div></label>
<input type="text" name="entry.896919823" value="" class="ss-q-short" id="entry_896919823" dir="auto" aria-label="Full Name  " aria-required="true" required="" title="">
<div class="error-message"></div>
<div class="required-message">This is a required question</div>
</div></div></div> <div class="ss-form-question errorbox-good" role="listitem">
<div dir="ltr" class="ss-item ss-item-required ss-text"><div class="ss-form-entry">
<label class="ss-q-item-label" for="entry_1183201344"><div class="ss-q-title">Job Title
<label for="itemView.getDomIdToLabel()" aria-label="(Required field)"></label>
<span class="ss-required-asterisk">*</span></div>
<div class="ss-q-help ss-secondary-text" dir="ltr"></div></label>
<input type="text" name="entry.1183201344" value="" class="ss-q-short" id="entry_1183201344" dir="auto" aria-label="Job Title  " aria-required="true" required="" title="">
<div class="error-message"></div>
<div class="required-message">This is a required question</div>
</div></div></div> <div class="ss-form-question errorbox-good" role="listitem">
<div dir="ltr" class="ss-item ss-item-required ss-text"><div class="ss-form-entry">
<label class="ss-q-item-label" for="entry_1436203379"><div class="ss-q-title">E-Mail
<label for="itemView.getDomIdToLabel()" aria-label="(Required field)"></label>
<span class="ss-required-asterisk">*</span></div>
<div class="ss-q-help ss-secondary-text" dir="ltr">The point of contact will receive an email to verify the information</div></label>
<input type="text" name="entry.1436203379" value="" class="ss-q-short" id="entry_1436203379" dir="auto" aria-label="E-Mail The point of contact will receive an email to verify the information " aria-required="true" required="" title="">
<div class="error-message"></div>
<div class="required-message">This is a required question</div>
</div></div></div> <div class="ss-form-question errorbox-good" role="listitem">
<div dir="ltr" class="ss-item  ss-text"><div class="ss-form-entry">
<label class="ss-q-item-label" for="entry_1905428539"><div class="ss-q-title">Telephone
</div>
<div class="ss-q-help ss-secondary-text" dir="ltr"></div></label>
<input type="text" name="entry.1905428539" value="" class="ss-q-short" id="entry_1905428539" dir="auto" aria-label="Telephone  " title="">
<div class="error-message"></div>
<div class="required-message">This is a required question</div>
</div></div></div> <div class="ss-form-question errorbox-good" role="listitem">
<div dir="ltr" class="ss-item ss-item-required ss-text"><div class="ss-form-entry">
<label class="ss-q-item-label" for="entry_1047757859"><div class="ss-q-title">Electronic Signature
<label for="itemView.getDomIdToLabel()" aria-label="(Required field)"></label>
<span class="ss-required-asterisk">*</span></div>
<div class="ss-q-help ss-secondary-text" dir="ltr">Please type "I AGREE" to agree to all the above terms.</div></label>
<input type="text" name="entry.1047757859" value="" class="ss-q-short" id="entry_1047757859" dir="auto" aria-label="Electronic Signature Please type &quot;I AGREE&quot; to agree to all the above terms. Must contain I AGREE" aria-required="true" required="" pattern=".*I AGREE.*" title="Must contain I AGREE">
<div class="error-message">Must contain I AGREE</div>
<div class="required-message">This is a required question</div>
</div></div></div>
<input type="hidden" name="draftResponse" value="[,,&quot;-2520726189736755125&quot;]
">
<input type="hidden" name="pageHistory" value="0">

<input type="hidden" name="fbzx" value="-2520726189736755125">

<div class="ss-item ss-navigate"><table id="navigation-table"><tbody><tr><td class="ss-form-entry goog-inline-block" id="navigation-buttons" dir="ltr">
<input type="submit" name="submit" value="Submit" id="ss-submit">
<div class="ss-password-warning ss-secondary-text">Never submit passwords through Google Forms.</div></td>
</tr></tbody></table></div></ol></form></div>
<div class="ss-footer"><div class="ss-attribution"></div>
<div class="ss-legal"><div class="disclaimer-separator"></div>
<div class="disclaimer" dir="ltr"><div class="powered-by-logo"><span class="powered-by-text">Powered by</span>
<div class="ss-logo-container"><span class="aria-only-help">Google Forms</span></div></div>
<div class="ss-terms"><span class="disclaimer-msg"></span>
<br>
<a href="https://docs.google.com/forms/d/1MWdnKZHfSiIf8OW0TvCLZh_yStLkxulozjjZEPUTOmU/reportabuse?source=https://docs.google.com/forms/d/1MWdnKZHfSiIf8OW0TvCLZh_yStLkxulozjjZEPUTOmU/viewform?embedded%3Dtrue">Report Abuse</a>
-
<a href="http://www.google.com/accounts/TOS">Terms of Service</a>
-
<a href="http://www.google.com/google-d-s/terms.html">Additional Terms</a></div></div></div></div>

<div id="docs-aria-speakable" class="docs-a11y-ariascreenreader-speakable docs-offscreen" aria-live="assertive" role="region" aria-atomic="" aria-relevant="additions">Screen reader support enabled. </div></div>

<script type="text/javascript" src="/cla/2292651181-formviewer_prd.js"></script>
<script type="text/javascript">H5F.setup(document.getElementById('ss-form'));_initFormViewer(
        "[100,\x22#CCC\x22,[[[1047757859,[[2,100,[\x22I AGREE\x22]\n,\x22\x22]\n]\n]\n]\n]\n]\n");
</script>
