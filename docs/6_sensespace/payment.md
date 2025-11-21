# Agent Payment API Documentation

This document describes the API endpoints for managing agent-specific payments within the SenseSpace platform.

Before your agents could charge from a user, you must complete KYA(Kown Your Agent) on [Verisense dashboard](https://dashboard.verisense.network).

Host: https://api.sensespace.xyz/

## 1. Initialize Payment Intent

-   **Endpoint:** `POST /v1/agent-payment/intent`
-   **Description:** Creates a payment intent for a transaction between a user and an agent. This endpoint verifies the agent's payment setup and the user's payment method before creating an intent. It might immediately charge the user or require further confirmaion.
-   **Authorization:** `Bearer <AGENT_API_KEY>` (The API key is hashed and used to identify the agent.)
-   **Request Body (`CreatePaymentIntentParams`):**
    ```json
    {
        "amount": 1000,
        "userId": "user_id_string"
    }
    ```
    -   `amount` (integer, required): The amount to be charged, in the smallest currency unit (e.g., cents for USD). Must be greater than 0.
    -   `userId` (string, required): The unique identifier for the user initiating the payment.

-   **Responses:**

    -   **200 OK:** Successfully created the payment intent or identified that a payment method is needed.

        *Case 1: User needs to add a payment method.* This typically occurs if the user has no payment methods linked.

        ```json
        {
            "success": true,
            "data": {
                "needPaymentMethod": true,
                "paymentIntent": null
            }
        }
        ```

        *Case 2: Payment intent created successfully, awaiting confirmation.* The `paymentIntentId` can be used for the confirmation step.

        ```json
        {
            "success": true,
            "data": {
                "needPaymentMethod": false,
                "paymentIntent": {
                    "status": "requires_confirmation",
                    "paymentIntentId": "pi_xxxxxxxxxxxx"
                }
            }
        }
        ```

        *Case 3: Payment intent succeeded directly for small amounts.* For very small transaction amounts, Stripe might directly succeed the payment without requiring explicit confirmation.

        ```json
        {
            "success": true,
            "data": {
                "needPaymentMethod": false,
                "paymentIntent": {
                    "status": "succeeded",
                    "paymentIntentId": "pi_xxxxxxxxxxxx"
                }
            }
        }
        ```


    -   **400 Bad Request:** Invalid input or agent payment status issue.
    -   **401 Unauthorized:** The agent's API key is missing or invalid.
    -   **500 Internal Server Error:** A server-side error occurred while processing the payment.



## 2. Confirm Payment Intent

-   **Endpoint:** `POST /v1/agent-payment/intent/{id}`
-   **Description:** Confirms and processes a payment intent that is in a `requires_confirmation` state. This action finalizes the charge and transfers the funds from the user's payment method.
-   **Authorization:** `Bearer <AGENT_API_KEY>` (The API key is hashed and used to identify the agent.)
-   **Path Parameters:**
    -   `id` (string, required): The ID of the payment intent to confirm (e.g., `pi_xxxxxxxxxxxx`). This ID is obtained from the `init_payment` response.
-   **Request Body (`ConfirmPaymentIntentParams`):**
    ```json
    {
        "confirmCode": "123456"
    }
    ```
    -   `confirmCode` (string, required): The confirmation code (e.g., a one-time password) provided by the user to authorize the payment. This code is checked against the `confirm_code` stored in the payment intent.

-   **Responses:**

    -   **200 OK:** Payment intent confirmed successfully. The `status` will be updated (e.g., `succeeded`, `requires_action` if 3D Secure is needed).

        ```json
        {
            "success": true,
            "data": {
                "needPaymentMethod": false,
                "paymentIntent": {
                    "status": "succeeded",
                    "paymentIntentId": "pi_xxxxxxxxxxxx"
                }
            }
        }
        ```

    -   **400 Bad Request:**
    -   **401 Unauthorized:** The agent's API key is missing or invalid.
    -   **500 Internal Server Error:** A server-side error occurred while confirming the payment.

