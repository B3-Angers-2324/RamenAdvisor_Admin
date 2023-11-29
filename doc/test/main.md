# **Functional Test for RamenAdvisor Navigation Bar:**

## Test 1: Initial Display

1. The "RamenAdvisor" logo must be displayed.
2. Navigation bar buttons should include "Edit Food Type," "Pending Owners List," "Reported Comments," "Ban Users," and a "Disconnect" button

## Test 2: "Edit Food Type" Button

1. Clicking the "Edit Food Type" button should display a page allowing the modification of food types.

## Test 3: "Pending Owners List" Buttons

1. Clicking on each "Owner name" button should display the profile information of the respective owner awaiting validation.

### Owner Profile Display

- Nom* :
- Prénom* :
- Email personnel* :
- Nom de l'entreprise* :
- Adresse du siège social* :
- Numéro de Siret* :
- Numéro de téléphone personnel* :

2. There should be "Validate" and "Reject" buttons for each owner.
   - Clicking the "Validate" button (checkmark icon) should approve the owner, changes should be successfully saved, and the owner should disapear from the list.
   - Clicking the "Reject" button (cross icon) should reject the owner, changes should be successfully saved, and the owner should disapear from the list.

## Test 4: "Reported Comments" Button

1. Clicking the "Reported Comments" button should display a list of reported comments.
2. The list should contain reported comments, and there should be an option to moderate each comment (e.g., delete or hide the comment).
3. Moderating a comment should be possible, and changes should be successfully saved.

## Test 5: "Ban Users" Button

1. Clicking the "Ban Users" button should display a list of registered users on RamenAdvisor.
2. The list should contain "nom", "prénom" and the number of sanctions.
4. The list is dynamically sorted based on the maximum number of sanctions.
5. When clicked on the card, the user's profile should be displayed.

### Ban User Page

1. The user's profile should contain the following information:
- Nom :
- Prénom :
- Mail :
- Date de naissance :

2. The user's profile should contain a list of comments made by the user.
3. Each comment could be deleted by clicking the "Delete Comment" button.

#### Actions

- Clicking the "X" button should refuse the ban request, changes should be successfully saved, and the owner should disapear from the list.
- Clicking the "V" button should accept the ban request, changes should be successfully saved, and the owner should disapear from the list.


## Test 6: "Disconnect" Button

1. Clicking the "Disconnect" button should log the user out of RamenAdvisor.
2. The user should be redirected to the login page or the home page after logout.
3. After logout, certain functionalities specific to the logged-in user (e.g., accessing the pending owners list) should no longer be accessible.
