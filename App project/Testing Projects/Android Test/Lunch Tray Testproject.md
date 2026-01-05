```
package com.example.lunchtray

imports
	import androidx.annotation.StringRes
	import androidx.compose.foundation.layout.fillMaxSize
	import androidx.compose.foundation.layout.padding
	import androidx.compose.material.icons.Icons
	import androidx.compose.material.icons.automirrored.filled.ArrowBack
	import androidx.compose.material3.ExperimentalMaterial3Api
	import androidx.compose.material3.Icon
	import androidx.compose.material3.IconButton
	import androidx.compose.material3.Scaffold
	import androidx.compose.material3.Text
	import androidx.compose.material3.TopAppBar
	import androidx.compose.runtime.Composable
	import androidx.compose.runtime.collectAsState
	import androidx.compose.runtime.getValue
	import androidx.compose.ui.Modifier
	import androidx.compose.ui.res.dimensionResource
	import androidx.compose.ui.res.stringResource
	import androidx.compose.ui.tooling.preview.Preview
	import androidx.lifecycle.viewmodel.compose.viewModel
	import androidx.navigation.NavHostController
	import androidx.navigation.compose.NavHost
	import androidx.navigation.compose.composable
	import androidx.navigation.compose.currentBackStackEntryAsState
	import androidx.navigation.compose.rememberNavController
	import com.example.lunchtray.datasource.DataSource
	import com.example.lunchtray.ui.AccompanimentMenuScreen
	import com.example.lunchtray.ui.CheckoutScreen
	import com.example.lunchtray.ui.EntreeMenuScreen
	import com.example.lunchtray.ui.OrderViewModel
	import com.example.lunchtray.ui.SideDishMenuScreen
	import com.example.lunchtray.ui.StartOrderScreen

enum class LunchTrayScreen(@StringRes val title: Int) {
    Start(R.string.app_name),
    Entree(R.string.choose_entree),
    Side(R.string.choose_side_dish),
    Accompaniment(R.string.choose_accompaniment),
    Checkout(R.string.order_checkout)
}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun LunchTrayAppBar(
    currentScreen: LunchTrayScreen,
    canNavigateBack: Boolean,
    navigateUp: () -> Unit = {},
    modifier: Modifier = Modifier,
) {
    TopAppBar(
        title = { Text(stringResource(currentScreen.title)) },
        modifier = modifier,
        navigationIcon = {
            if (canNavigateBack) {
                IconButton(onClick = navigateUp) {
                    Icon(
                        imageVector = Icons.AutoMirrored.Filled.ArrowBack,
                        contentDescription = stringResource(R.string.back_button)
                    )
                }
            }
        }
    )

}

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun LunchTrayApp(
    navController: NavHostController = rememberNavController(),
) {

    // Create ViewModel
    val viewModel: OrderViewModel = viewModel()
    val backStackEntry by navController.currentBackStackEntryAsState()
    val currentScreen = LunchTrayScreen.valueOf(
        backStackEntry?.destination?.route ?: LunchTrayScreen.Start.name
    )

    Scaffold(
        topBar = {
            LunchTrayAppBar(
                currentScreen = currentScreen,
                canNavigateBack = navController.previousBackStackEntry != null,
                navigateUp = { navController.navigateUp() }
            )
        }
    ) { innerPadding ->
        val uiState by viewModel.uiState.collectAsState()
        NavHost(
            navController = navController,
            startDestination = LunchTrayScreen.Start.name,
            modifier = Modifier.padding(innerPadding)
        ) {
            composable(route = LunchTrayScreen.Start.name) {
                StartOrderScreen(
                    onStartOrderButtonClicked = {
                        navController.navigate(LunchTrayScreen.Entree.name)
                    },
                    modifier = Modifier
                        .fillMaxSize()
                        .padding(dimensionResource(R.dimen.padding_medium))
                )
            }
            composable(route = LunchTrayScreen.Entree.name) {
                EntreeMenuScreen(
                    options = DataSource.entreeMenuItems,
                    onCancelButtonClicked = {
                        cancelOrderAndNavigateToStart(
                            viewModel,
                            navController
                        )
                    },
                    onNextButtonClicked = {
                        navController.navigate(route = LunchTrayScreen.Side.name)
                    },
                    onSelectionChanged = { viewModel.updateEntree(it) },
                    modifier = Modifier
                        .fillMaxSize()
                        .padding(dimensionResource(R.dimen.padding_medium))
                )
            }
            composable(route = LunchTrayScreen.Side.name) {
                SideDishMenuScreen(
                    options = DataSource.sideDishMenuItems,
                    onCancelButtonClicked = {
                        cancelOrderAndNavigateToStart(
                            viewModel,
                            navController
                        )
                    },
                    onNextButtonClicked = {
                        navController.navigate(route = LunchTrayScreen.Accompaniment.name)
                    },
                    onSelectionChanged = { viewModel.updateSideDish(it) },
                    modifier = Modifier
                        .fillMaxSize()
                        .padding(dimensionResource(R.dimen.padding_medium))
                )
            }
            composable(route = LunchTrayScreen.Accompaniment.name) {
                AccompanimentMenuScreen(
                    options = DataSource.accompanimentMenuItems,
                    onCancelButtonClicked = {
                        cancelOrderAndNavigateToStart(
                            viewModel,
                            navController
                        )
                    },
                    onNextButtonClicked = {
                        navController.navigate(route = LunchTrayScreen.Checkout.name)
                    },
                    onSelectionChanged = { viewModel.updateAccompaniment(it) },
                    modifier = Modifier
                        .fillMaxSize()
                        .padding(dimensionResource(R.dimen.padding_medium))
                )
            }
            composable(route = LunchTrayScreen.Checkout.name) {
                CheckoutScreen(
                    orderUiState = uiState,
                    onNextButtonClicked = {
                        cancelOrderAndNavigateToStart(
                            viewModel,
                            navController
                        )
                    },
                    onCancelButtonClicked = {
                        cancelOrderAndNavigateToStart(
                            viewModel,
                            navController
                        )
                    }
                )
            }
        }
    }
}

private fun cancelOrderAndNavigateToStart(
    viewModel: OrderViewModel,
    navController: NavHostController,
) {
    viewModel.resetOrder()
    navController.popBackStack(LunchTrayScreen.Start.name, inclusive = false)
}

@Preview
@Composable
fun ScreenPreviev() {
    LunchTrayApp()
}
```