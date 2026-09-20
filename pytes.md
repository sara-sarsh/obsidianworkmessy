How to use pytest:
Superpowers: Testing + Mocks Cheat Sheet
	Use MagicMock() for making complex mocks (with attributes, chained calls).
	Use patch() to swap out dependencies for mocks.
	Use .assert_called_once_with(...) to check if a mock was called.
	Parametrize tests in pytest with @pytest.mark.parametrize for broad coverage.