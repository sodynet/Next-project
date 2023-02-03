# devops-capstone-project
    def test_get_account_list(self):
        """It should Get a list of Accounts"""
        self._create_accounts(5)
        resp = self.client.get(BASE_URL)
        self.assertEqual(resp.status_code, status.HTTP_200_OK)
        data = resp.get_json()
        self.assertEqual(len(data), 5)
        
            ######################################################################
    # LIST ALL ACCOUNTS
    ######################################################################
    @app.route("/accounts", methods=["GET"])
    def list_accounts():
        """
        List all Accounts
        This endpoint will list all Accounts
        """
        app.logger.info("Request to list Accounts")

        accounts = Account.all()
        account_list = [account.serialize() for account in accounts]

        app.logger.info("Returning [%s] accounts", len(account_list))
        return jsonify(account_list), status.HTTP_200_OK
        
            def test_update_account(self):
        """It should Update an existing Account"""
        # create an Account to update
        test_account = AccountFactory()
        resp = self.client.post(BASE_URL, json=test_account.serialize())
        self.assertEqual(resp.status_code, status.HTTP_201_CREATED)

        # update the account
        new_account = resp.get_json()
        new_account["name"] = "Something Known"
        resp = self.client.put(f"{BASE_URL}/{new_account['id']}", json=new_account)
        self.assertEqual(resp.status_code, status.HTTP_200_OK)
        updated_account = resp.get_json()
        self.assertEqual(updated_account["name"], "Something Known")
        
            ######################################################################
    # UPDATE AN EXISTING ACCOUNT
    ######################################################################
    @app.route("/accounts/<int:account_id>", methods=["PUT"])
    def update_accounts(account_id):
        """
        Update an Account
        This endpoint will update an Account based on the posted data
        """
        app.logger.info("Request to update an Account with id: %s", account_id)

        account = Account.find(account_id)
        if not account:
            abort(status.HTTP_404_NOT_FOUND, f"Account with id [{account_id}] could not be found.")

        account.deserialize(request.get_json())
        account.update()

        return account.serialize(), status.HTTP_200_OK
        
            def test_delete_account(self):
        """It should Delete an Account"""
        account = self._create_accounts(1)[0]
        resp = self.client.delete(f"{BASE_URL}/{account.id}")
        self.assertEqual(resp.status_code, status.HTTP_204_NO_CONTENT)
        
            ######################################################################
    # DELETE AN ACCOUNT
    ######################################################################
    @app.route("/accounts/<int:account_id>", methods=["DELETE"])
    def delete_accounts(account_id):
        """
        Delete an Account
        This endpoint will delete an Account based on the account_id that is requested
        """
        app.logger.info("Request to delete an Account with id: %s", account_id)

        account = Account.find(account_id)
        if account:
            account.delete()

        return "", status.HTTP_204_NO_CONTENT
        
        

